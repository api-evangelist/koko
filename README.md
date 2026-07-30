# Koko

Koko is a 501(c)(3) nonprofit that makes free, evidence-based mental health support available to young people inside the online platforms they already use. Its model is detection plus direction: a keyword and AI engine identifies high-risk posts and searches on partner platforms, and matched users are routed to free crisis helplines and self-guided mini-courses. Koko has been deployed by TikTok, Pinterest, Giphy, Snapchat, Tumblr, Bluesky, WhatsApp and Discord.

Koko ships this as a developer-facing Suicide Prevention Toolkit built on two APIs:

- **Koko Keywords API** (`https://api.kokocares.org/keywords`) — credential-gated keyword matching against Koko's risk taxonomy across category, confidence and intensity. Consumed through a native Rust client with Python, Ruby, Go and PHP bindings.
- **Crisis Helplines API** (`https://helpline-api.koko.ai`) — a public, unauthenticated JSON API returning crisis helpline data by country. Access is gated by a server-side CORS origin allowlist.

- Website: https://kokocares.org
- Developer docs: https://developers.kokocares.org/docs/getting-started
- GitHub: https://github.com/kokocares

Backed by: union-square-ventures

> **Domain note.** This profile was created from a VC-portfolio stub that recorded `itskoko.com` as the website. As of 2026-07-19 that domain no longer belongs to Koko — it serves an unrelated cricket betting affiliate site. The live Koko surface is `kokocares.org` (legal entity Koko AI Inc.).
