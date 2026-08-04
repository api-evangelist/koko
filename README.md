# Koko

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Koko is a 501(c)(3) nonprofit that makes free, evidence-based mental health support available to young people inside the online platforms they already use. Its model is detection plus direction: a keyword and AI engine identifies high-risk posts and searches on partner platforms, and matched users are routed to free crisis helplines and self-guided mini-courses. Koko has been deployed by TikTok, Pinterest, Giphy, Snapchat, Tumblr, Bluesky, WhatsApp and Discord.

Koko ships this as a developer-facing Suicide Prevention Toolkit built on two APIs:

- **Koko Keywords API** (`https://api.kokocares.org/keywords`) — credential-gated keyword matching against Koko's risk taxonomy across category, confidence and intensity. Consumed through a native Rust client with Python, Ruby, Go and PHP bindings.
- **Crisis Helplines API** (`https://helpline-api.koko.ai`) — a public, unauthenticated JSON API returning crisis helpline data by country. Access is gated by a server-side CORS origin allowlist.

- Website: https://kokocares.org
- Developer docs: https://developers.kokocares.org/docs/getting-started
- GitHub: https://github.com/kokocares

Backed by: union-square-ventures

> **Domain note.** This profile was created from a VC-portfolio stub that recorded `itskoko.com` as the website. As of 2026-07-19 that domain no longer belongs to Koko — it serves an unrelated cricket betting affiliate site. The live Koko surface is `kokocares.org` (legal entity Koko AI Inc.).
