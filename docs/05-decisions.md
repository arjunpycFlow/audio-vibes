# Decisions log

Each decision is Proposed until Arjun and the owners confirm it.

## ADR-001: Site stack — Astro static site on GitHub Pages

Status: Proposed (2026-10-25) — confirm with Arjun and owners.

## Context
- Repo and custom domain already run on GitHub Pages (`CNAME` = audio-vibes.com).
- The site is mostly content: services, packages, gear, photos, FAQ, a quote form. No accounts, no checkout in Phase 1.
- The builder (Arjun) is an engineer using Claude Code; the owners are not developers.
- Local-search visitors are on phones; speed and SEO matter more than fancy interactivity.

## Options
| Option | Pros | Cons |
|---|---|---|
| **Astro + GitHub Pages** | Free hosting already set up; very fast static pages; content as files Claude can edit; full SEO control; no lock-in | Owners need a person (or a git-based CMS) to edit content; forms need a 3rd-party backend |
| Wix / Squarespace (what Orlans uses) | Owners edit everything; built-in forms | Monthly fee; slower mobile; less SEO/structure control; not a Claude Code project |
| Webflow (what PCE uses) | Polished design, CMS | Cost; learning curve; designer-oriented |
| Booqable website builder | Built for rentals; availability + payments | Monthly cost; template-bound; better added later for the rental flow only |

## Decision
Astro (latest stable) static site, deployed by GitHub Actions with `withastro/action` to GitHub Pages. `public/CNAME` holds the domain; `site: "https://audio-vibes.com"`, no `base`. Repo Settings → Pages → Source must be switched to "GitHub Actions".

Content in content collections (see 06-content-model.md). Plain CSS + tokens. A git-based CMS (e.g. Pages CMS or Decap) can be added later if owners want to edit gear/prices themselves — decide in a later ADR.

Forms: a static form backend (candidates: Web3Forms, Formspree, or a Cloudflare Worker) — record as ADR-003 after checking current free-tier limits and spam protection.

## Consequences
- Near-zero hosting cost; Lighthouse-friendly by default.
- Rental software (Phase 3) will be embedded or linked, not rebuilt.

Reference: [Astro — Deploy to GitHub Pages](https://docs.astro.build/ja/guides/deploy/github) (workflow uses `actions/checkout`, `withastro/action`, `actions/deploy-pages`; check current major versions when writing the workflow).

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
