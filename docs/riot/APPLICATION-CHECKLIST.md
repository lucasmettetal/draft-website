# DRAFT — Production application checklist

Status as of 2026-11-07. ✅ done · ⚠️ partial · ❌ missing.

| Item | Status | Notes |
|---|---|---|
| Public website | ✅ | https://draft-website-ashy.vercel.app (FR) and `/en/` (EN) |
| Domain controlled by the developer | ⚠️ | Vercel subdomain only. A custom domain is recommended before applying; update canonical/hreflang URLs if it changes. |
| `riot.txt` verification | ❌ | Only when Riot provides the exact token: create `riot.txt` at the site root with that content, deploy, check `https://<domain>/riot.txt`. **Do not create it before.** |
| Product description | ✅ | [PRODUCT-OVERVIEW.md](PRODUCT-OVERVIEW.md); site sections Features / How it works |
| API usage description | ✅ | [API-USAGE.md](API-USAGE.md); site section "Riot API" |
| Presentation video | ⚠️ | 4-min tutorial with FR voice and FR/EN subtitles, on the site (`#demo`). An English voice-over would help reviewers. |
| Screenshots | ✅ | `assets/` — DRAFT 2.16 interface, simulated draft, fictitious data |
| Privacy policy | ✅ | `/confidentialite.html`, `/en/privacy.html` (technical description; no legal/GDPR review done) |
| Terms of use | ✅ | `/conditions.html`, `/en/terms.html` |
| Contact | ✅ | lucas.mettetal2@gmail.com, GitHub Issues |
| Publisher identity | ❌ | No legal name, status or address published. Decide what to publish (French law may require publisher information for a public site); do not invent it. |
| Riot legal boilerplate | ✅ | Footer of every page |
| Key security / architecture | ⚠️ | Current: user's own dev key, never stored. Production: server holding the key — **designed, not built** ([API-USAGE.md](API-USAGE.md)). |
| Riot Sign On for personal history | ❌ | Requires a Production key first; planned. |
| LCU use declared | ⚠️ | Described on the site; must be declared on the Developer Portal product page. |
| Working prototype | ⚠️ | 2.16 beta builds and passes automated tests; not yet tested by external players; not pushed to GitHub (`main` = 2.14). |
| Answers to compliance questions | ❌ | [COMPLIANCE-QUESTIONS.md](COMPLIANCE-QUESTIONS.md) |
| Code signing | ❌ | Executable unsigned (Windows SmartScreen warning). |
| License | ❌ | No license file in the app repository. |
