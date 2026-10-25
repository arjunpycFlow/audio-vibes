# Strategy and roadmap

## The core idea

A local event-services business gets customers in this order: **someone searches (Google/Maps/Yelp/Instagram) → sees proof (photos, reviews) → gets a feel for price → contacts you**. The website covers the last three steps; Google Business Profile and reviews cover the first. Building the site without the profile is the most common mistake for a business like this.

**Positioning (draft — confirm with owners):**
> Great sound for small and mid-size events in OC & LA. Rent the gear, or let us run the show. You talk directly to the people doing the work.

## Phase 0 — Foundations (week 1, mostly not code)

- [ ] Run the Kickoff playbook with the owners; close the critical items in 04-open-questions.md.
- [ ] Business email on the domain (e.g. `book@audio-vibes.com`) — needed for forms and profiles.
- [ ] **Google Business Profile**: service-area business if there is no storefront customers visit (Google requires hiding the address in that case; up to 20 service areas; keep within ~2 hours drive). Categories, services, photos, phone. ([Google guidelines](https://support.google.com/business/answer/10514137?hl=en))
- [ ] Yelp business page; Instagram handle; consistent name/phone everywhere (NAP).
- [ ] Photo shoot: gear on a clean background + 3–5 real events (with permission).
- [ ] Ask 5–10 past customers for Google reviews.

## Phase 1 — Launch site (weeks 1–3)

Pages (MVP):
1. **Home** — one-line promise, two big doors (Rent gear / Full production), packages teaser, proof strip, how it works (3 steps), quote form, call/text.
2. **Rentals** — packages by use case (Party, Speech/Meeting, Live Band, DJ) with "starting at" prices and guest-count guidance; à la carte gear list by category; service levels table (pickup / delivery & setup / full service).
3. **Production** — what "we run your sound" includes, event types, process, example events, quote CTA.
4. **Gallery / Our work** — real events, short captions.
5. **About** — the owners, faces, why they started.
6. **FAQ & policies** — deposits, damage, cancellation, delivery fees, outdoor/power, lead time.
7. **Contact / Get a quote** — form with event date, location (city), guest count, indoor/outdoor, service level, gear needs.

Technical: Astro + content collections, JSON-LD `LocalBusiness`, sitemap, robots, OG images, analytics, form backend, 404, Lighthouse checks in CI.

**Quote list:** visitors can "add to quote" packages/items (stored in the browser), and the list is attached to the quote form. No online payment in Phase 1.

## Phase 2 — Grow (months 1–3)

- Location/segment landing pages only where there's real demand and something real to say (e.g. "Wedding sound rental in Orange County", "PA rental Los Angeles"). No thin doorway pages.
- Event stories (one per real event: what, where, gear, photo, quote from client).
- Review widget or curated review quotes (with permission, attributed).
- Listings on wedding/event marketplaces if weddings are a target segment.
- Monthly check: Search Console queries, GBP calls/website clicks, form submissions → adjust.

## Phase 3 — Operate (when volume justifies it)

- Rental software with live availability, deposits and contracts (see 05-decisions.md, ADR-002). Trigger: double-bookings, or >~10 rental inquiries/week, or owners spending hours on availability texts.
- Online deposits; e-signature rental agreement; automated reminders.
- Repeat-customer offers for venues and DJs.

## Metrics that matter

Quote requests per week (form + calls + texts) · booked jobs · conversion rate quote→booked · Google reviews count/rating · GBP calls and website clicks · top search queries.
