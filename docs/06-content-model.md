# Content model

For the developer / Claude Code side. In this project it is a reference for how owner facts should be captured so they can drop straight into the site later.

All business content lives in `src/content/` (Astro content collections) or `src/data/` so owners' facts are edited in one place and pages are generated from them. Schemas are defined with Zod in `src/content.config.ts`; `npm run check` validates them.

## `src/data/site.ts` — single source of truth for NAP

```ts
export const site = {
  name: "Audio Vibes",
  url: "https://audio-vibes.com",
  email: "TODO(owner)",
  contacts: [ { name: "Baba", phone: "..." }, { name: "Vinny", phone: "..." } ], // from current index.html
  serviceAreas: ["Orange County", "Los Angeles"],   // refine with owners
  hours: "TODO(owner)",
  social: { instagram: "", yelp: "", google: "" },
};
```
Every phone/email/area shown on the site and in JSON-LD comes from here.

## Collection: `gear` (one file per item, `src/content/gear/qsc-k12-2.md`)

| Field | Type | Notes |
|---|---|---|
| name | string | "QSC K12.2 powered speaker" |
| category | enum | speakers, subwoofers, mixers, microphones, wireless, dj, monitors, stands-cables, lighting-addons, power |
| brand / model | string | |
| plainUse | string | What it does for the event, in plain words |
| specs | list of strings | Short, secondary |
| pricePerDay | number? | Omit → "Call for price" |
| quantityOwned | number? | Internal planning; not shown |
| image | image | Real photo preferred |
| featured | boolean | |
| status | enum | available, retired |

## Collection: `packages`

| Field | Type | Notes |
|---|---|---|
| name | string | "Party Pack — Medium" |
| useCase | enum | party, speech-meeting, live-band, dj, ceremony, outdoor |
| guestsUpTo | number | Guidance, e.g. 150 |
| indoorOutdoor | enum | indoor, outdoor, both |
| includes | list of gear refs + free text | |
| priceFrom | number | "Starting at", per day, dry hire |
| serviceLevels | list | pickup, delivery-setup, full-service |
| order | number | Sort order |

## Collection: `services` — Production, Delivery & Setup, etc. (title, summary, includes[], process steps[], faq[]).

## Collection: `events` (portfolio)

title, date (month/year ok), venueType, city, guests, serviceLevel, packageUsed?, photos[], clientQuote?, clientName? (only with permission), published.

## Collection: `faq` — question, answer, category (rentals, production, payments, policies).

## Quote request (form payload)

name, email, phone, preferred contact (call/text/email), event date, event city, venue type, indoor/outdoor, guest count, service level, items in quote list, message. Spam protection: honeypot + the provider's captcha if needed.
