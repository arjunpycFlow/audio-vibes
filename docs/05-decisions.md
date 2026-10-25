# Decisions log

Each decision is Proposed until Arjun and the owners confirm it.

## ADR-001: Site stack — Astro static site on GitHub Pages

Status: Proposed (2026-10-25) — confirm with Arjun and owners.

## Context
- The site will be hosted on GitHub Pages (repo: see ADR-004).
- The site is mostly content: services, packages, gear, photos, FAQ, a quote form. No accounts, no checkout in Phase 1.
- The builder (Arjun) is an engineer using Claude Code; the owners are not developers.
- Local-search visitors are on phones; speed and SEO matter more than fancy interactivity.

## Options
| Option | Pros | Cons |
|---|---|---|
| **Astro + GitHub Pages** | Free hosting; very fast static pages; content as files Claude can edit; full SEO control; no lock-in | Owners need a person (or a git-based CMS) to edit content; forms need a 3rd-party backend |
| Wix / Squarespace (what Orlans uses) | Owners edit everything; built-in forms | Monthly fee; slower mobile; less SEO/structure control; not a Claude Code project |
| Webflow (what PCE uses) | Polished design, CMS | Cost; learning curve; designer-oriented |
| Booqable website builder | Built for rentals; availability + payments | Monthly cost; template-bound; better added later for the rental flow only |

## Decision
Astro (latest stable) static site, deployed by GitHub Actions with `withastro/action` to GitHub Pages. `site: "https://audio-vibes.com"`. Until the custom domain is moved to the new repo, the site is served at a project path, so `base` is read from an environment variable (see ADR-004). Repo Settings → Pages → Source must be "GitHub Actions".

Content in content collections (see 06-content-model.md). Plain CSS + tokens. A git-based CMS (e.g. Pages CMS or Decap) can be added later if owners want to edit gear/prices themselves — decide in a later ADR.

Forms: see ADR-003.

## Consequences
- Near-zero hosting cost; Lighthouse-friendly by default.
- Rental software (Phase 3) will be embedded or linked, not rebuilt.

Reference: [Astro — Deploy to GitHub Pages](https://docs.astro.build/en/guides/deploy/github/) (workflow uses `actions/checkout`, `withastro/action`, `actions/deploy-pages`; check current major versions when writing the workflow).

## ADR-002: Rentals — quote-first now, rental software later

Status: Proposed (2026-10-25)

## Context
- Small inventory; one set of gear may serve both production jobs and rentals, so availability is easy to get wrong.
- Customers often don't know what they need. A conversation (or text) usually closes the sale.
- Observed: PCE uses a cart that leads to checkout/quote; Orlans shows prices but books by call/text/email (see 02-competitive-analysis.md).

## Decision
Phase 1: **quote list + quote form**. Show packages with "starting at" prices and à la carte items with day prices where the owners agree. "Add to quote" saves items in the browser; the form sends them with event details. No live availability, no online payment.

Phase 3 trigger to adopt rental software: double-bookings, ~10+ rental inquiries/week, or owners losing hours to availability/payment admin.

Candidates then: **Booqable** (website plugin with live availability and online payments; plans from about $29–35/mo as of Oct 2026, 14-day free trial — [pricing](https://booqable.com/pricing/)), **Goodshuffle Pro** (event-rental focused, quotes/contracts — [Capterra](https://www.capterra.com/p/167364/Goodshuffle-Pro/)), or Current RMS / Rentman for AV-specific workflows. Re-check prices at decision time.

## Consequences
- Simple to build and run now; owners keep control of pricing per job.
- Gear data model is kept clean (06-content-model.md) so it can be imported into rental software later.

## ADR-003: Form backend — Web3Forms free plan

Status: Proposed (2026-10-25) — confirm with Arjun.

## Context
- Static site (ADR-001) has no server, so the contact/quote form needs a hosted form-to-email service.
- Expected volume is small (well under 100 messages/month at launch).
- Spam protection matters: public forms on a business site attract bots.

## Options (checked 2026-10-25)
| Option | Free plan | Notes |
|---|---|---|
| **Web3Forms** | 250 submissions/month, unlimited forms, hCaptcha + spam filtering included, 30-day dashboard history; Pro ~$149/year | No account needed; access key is public by design; restrict to allowed domains. [Pricing](https://web3forms.com/pricing) |
| Formspree | 50 submissions/month, 30-day history; paid from ~$10–15/month | Mature product, smaller free plan. [Plans](https://formspree.io/plans) |
| Own Cloudflare Worker | Free tier, full control | More code and upkeep; worth it only when we need a database, auto-replies or CRM sync |

## Decision
Web3Forms free plan, submitting to the business email (needs Q5). Honeypot field on every form; turn on hCaptcha if spam appears. Form field names follow the quote-request payload in 06-content-model.md so the same backend serves the later quote form.

## Consequences
- $0/month. The owners' inbox is the record of leads, because the free dashboard keeps only 30 days.
- Upgrade trigger: over ~200 submissions/month, or the owners want auto-replies, Google Sheets, or webhooks → Web3Forms Pro or a Cloudflare Worker (new ADR).
- Swapping providers later means changing one endpoint in the form component, not the pages.

## ADR-004: Code repository — arjunpycFlow/audio-vibes

Status: Accepted (2026-10-25, Arjun)

## Context
- The old placeholder lives in `audio-vibes/audiovibes` (org repo) with `index.html`, `Logo.png`, the owners' phone numbers and the `CNAME` for audio-vibes.com.
- Arjun created a new, empty repo `arjunpycFlow/audio-vibes` under his personal account and chose it for all further work.

## Decision
All new work happens in `git@github.com:arjunpycFlow/audio-vibes.git`. The old org repo is reference only (logo, phone numbers) until the domain moves.

## Consequences
- `Logo.png` and the phone numbers must be copied from the old repo.
- Before the domain moves, GitHub Pages serves the new site at `https://arjunpycflow.github.io/audio-vibes/`, so Astro `base` comes from an env var (`/audio-vibes` now, `/` after the domain moves).
- Domain cut-over (at launch): remove the custom domain from the old repo's Pages settings → add `public/CNAME` and set the custom domain on the new repo → set `base` to `/` → confirm HTTPS.
- Ownership: the business site lives under Arjun's personal account. Consider transferring the repo to the `audio-vibes` org later so the owners own it (GitHub keeps redirects after a transfer).