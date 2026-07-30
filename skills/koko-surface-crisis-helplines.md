---
name: Surface crisis helplines for a country
description: >-
  Look up the crisis helplines Koko publishes for a given country and present them to a
  person in distress, verbatim and without paraphrase. Handles country aliases, the
  empty-result case, and the CORS origin allowlist that gates this API.
api: openapi/koko-crisis-helplines-openapi.yml
base_url: https://helpline-api.koko.ai
operations:
  - getCountries
  - getHelplines
generated: '2026-07-19'
method: generated
source: openapi/koko-crisis-helplines-openapi.yml
---

# Surface crisis helplines for a country

## Safety first — read before using this skill

This skill returns crisis-support contact details for people who may be at risk of
suicide or self-harm. Treat the returned data as **verbatim output, not source
material**:

- Never paraphrase, shorten, translate, or "clean up" a helpline name, phone number,
  short code, or URL. Reproduce `name`, `phone`, `displayNumber`, and `url` exactly.
- Never substitute a number you know from training data for one the API returned.
- If the lookup fails or returns nothing, say so plainly and hand off to a human or a
  general resource — do not improvise a helpline.
- This API provides referral information only. It does not provide clinical assessment
  and is not a substitute for emergency services.

## Prerequisites

- No credentials. The Crisis Helplines API is public (`security: []`).
- **But access is gated by a server-side CORS origin allowlist.** If your origin is not
  registered with Koko, every request returns HTTP `403` with a `text/plain` body
  (`Forbidden: no allowed origins configured` or
  `Forbidden: Origin <origin> not allowed`). Register the calling origin with Koko, or
  call server-side without an `Origin` header.

## Steps

### 1. Discover valid countries — `getCountries`

```
GET https://helpline-api.koko.ai/crisis_helplines/countries
```

Returns `{ "countries": [ { "countryCode": "US", "countryI18nKey": "United States" } ] }`,
sorted alphabetically by `countryI18nKey` and deduplicated. Only countries that have at
least one helpline appear here.

Use this to resolve the user's country to a value `getHelplines` will accept. Do this
first rather than guessing — country names are **case-sensitive** and must match stored
values exactly.

### 2. Fetch the helplines — `getHelplines`

```
GET https://helpline-api.koko.ai/crisis_helplines?country=United%20States
```

`country` is **required**. It accepts a full country name or a documented alias — `USA`
and `America` both resolve to `United States`, `CAN` resolves to `Canada`.

### 3. Handle the empty result correctly

An unknown country, or a country with no helpline data, returns **HTTP 200 with `[]`** —
not a 404. Branch on array length, never on status code. On an empty array, tell the
user no helpline data is available for their country and escalate to a human.

### 4. Filter and present

Each `Helpline` carries fields you can use to pick the most appropriate entry:

- `modality` — one of `phone`, `text`, `chat`, `email`. Match it to how the user is
  reaching out.
- `identity` — the demographic served (documented examples: `general`, `youth`, `lgbtq`,
  `veterans`, `indigenous`). Open vocabulary — do not assume the set is closed.
- `topic` — the issue area (documented examples: `crisis`, `suicide`,
  `domestic_violence`, `substance_abuse`, `mental_health`). Also open.
- `availability` / `hours` — e.g. `24/7`. Prefer a currently-available helpline. Note
  the schema itself says `hours` may duplicate `availability`.
- `topicMenu` — a boolean UI flag for whether the helpline belongs in categorized views.

Present `formattedCallNumber` for tel: links and `formattedTextingNumber` for sms:
links; use `displayNumber` / `displayURL` for human-readable display.

## Error handling

| Status | Body | Meaning | Action |
|---|---|---|---|
| 400 | `{"error": "Country required"}` | `country` omitted | Resolve a country via `getCountries` and retry |
| 403 | `text/plain` | Origin not on the allowlist | Register the origin with Koko; do not retry blindly |
| 200 | `[]` | No data for that country | Do not invent a helpline — escalate |

Errors are a flat `{"error": string}` object, **not** RFC 9457 `problem+json`, and the
403 is plain text. Do not assume a JSON body on every error. See
`errors/koko-problem-types.yml`.

## Gotchas

- **`countryI18nKey` means two different things.** On `Country` it is a full country name
  (`United States`); on `Helpline` it is a locale code (`en-US`). Do not join the two
  operations on this field — join on `countryCode`.
- No pagination, no rate-limit headers, and no request-id header are documented. See
  `conventions/koko-conventions.yml`.
