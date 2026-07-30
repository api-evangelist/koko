---
name: Match a search term against Koko's risk keyword taxonomy
description: >-
  Use the Koko Keywords API through an official client binding to detect whether a user
  search or post matches Koko's high-risk keyword taxonomy, then route a positive match
  to Koko's support resources. Covers credentials, the three-dimension filter syntax,
  and the referral step.
api: https://api.kokocares.org/keywords
base_url: https://api.kokocares.org/keywords
operations: []
operations_note: >-
  Koko publishes no OpenAPI for the Keywords API. Every step below is grounded in the
  published quickstart, the language-binding docs, and the client source at
  github.com/kokocares/keywords-client — nothing here is inferred from a spec.
generated: '2026-07-19'
method: generated
source: https://developers.kokocares.org/docs/quickstart
---

# Match a search term against Koko's risk keyword taxonomy

## Safety first

This skill decides whether someone may be at risk. Two failure modes matter in opposite
directions: a missed match leaves a person without support, and an over-eager match can
be intrusive or stigmatizing. Koko's documented guidance is to **start with high
confidence and high intensity on the highest-risk categories** and widen from there.
Never log or retain the matched user text beyond what your own policy allows.

## Prerequisites

1. **Get credentials.** Request an API key from Koko at
   <https://r.kokocares.org/api_signup>. There is no self-serve issuance.
2. **Set the environment variable.** The clients read HTTP Basic credentials from
   `KOKO_KEYWORDS_AUTH` in `username:password` form:

   ```
   export KOKO_KEYWORDS_AUTH=username:password
   ```

   It must be set **before importing the library** — otherwise the client raises
   `KOKO_KEYWORDS_AUTH must be set before importing the library`.
3. **Install a binding** (see `packages/koko-packages.yml`):
   - Python — `pip install koko_keywords` (PyPI `koko-keywords`)
   - Ruby — `gem install koko_keywords` (RubyGems `koko_keywords`)
   - Go — source only, `github.com/kokocares/keywords-client/clients/go`
   - PHP — source only, `github.com/kokocares/keywords-client-php` (requires `ext-FFI`)

## Steps

### 1. Match the term

```python
import koko_keywords

koko_keywords.match("sewerslide")
```

Returns a boolean. The underlying engine is a Rust core cross-compiled to four CPU
targets; it caches the compiled regex set based on the response cache-expiration headers
(currently one hour), keeping overhead under 1μs per request.

### 2. Apply a taxonomy filter

The optional second argument is a colon-delimited list of `dimension=value` filters
across three dimensions — **category**, **confidence**, **intensity**:

```python
koko_keywords.match("suicide", "category=suicide,self_harm,eating_disorder:confidence=high")
```

```python
koko_keywords.match("sewerslide", "category=eating_disorder,parenting:confidence=high,low")
```

**Omitting a dimension does not filter on it.** The second example matches against the
eating-disorder and parenting categories at high or low confidence, at any intensity.

An invalid filter raises: `Invalid filter, please ensure it follows the format:
category=value:another_category=value,value2`.

### 3. On a positive match, refer the user

Koko's documented pattern is to direct a matched user to Koko's support page rather than
to a hardcoded resource list, so the user always gets current resources. Format the
referral URL with your site name and the search term:

```
https://r.kokocares.org/<site_name>?q=suicide
```

For a programmatic helpline lookup instead of a redirect, use the Crisis Helplines API —
see `skills/koko-surface-crisis-helplines.md`.

## Error handling

The clients raise a runtime error with a remediation hint rather than returning error
codes; no exception handling is expected to be necessary in normal operation.

| Condition | Message |
|---|---|
| Missing credentials | `KOKO_KEYWORDS_AUTH must be set before importing the library` |
| Bad credentials (HTTP 401) | `Invalid credentials. Please confirm you are using valid credentials, contact us at api@kokocares.org if you need assistance.` |
| Cache refresh failure | `Unable to refresh cache. Please try again or contact us at api@kokocares.org if this issue persists.` |
| Unparseable response | `Unable to parse response from API. Please contact us at api@kokocares.org if this issue persists.` |
| Bad URL | `Invalid url. Please ensure the url used is valid.` |
| Bad filter | `Invalid filter, please ensure it follows the format: category=value:another_category=value,value2` |

As of client 0.2.0 the API and clients are fault tolerant — a transient upstream failure
falls back to the cached keyword set rather than failing the call. Minimal log messages
go to STDERR.

## Notes

- The client sends `X-API-VERSION: v3` to `https://api.kokocares.org/keywords`.
- Support contact for API issues: `api@kokocares.org`.
- See `authentication/koko-authentication.yml` and `conventions/koko-conventions.yml`.
