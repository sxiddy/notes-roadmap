Status: TODO

# C2 "N students unlocked" and review stars on subject cards
Value high · Cost small (about 4%) · Payment code: read-only use of sales counts (safe) · Order: 2

## Buyer benefit
A visitor sees that other students already bought and what they rated it.

## Design
- Counts: `GET /api/session` already lists products. Add `buyers` per product = `HLEN paid:<CODE>` (one pipeline,
  cache 5 minutes in memory). Show only when 10 or more ("120 students have Pro"); below that show nothing.
  Owner decides whether manual grants count (default: count all live records).
- Stars: `GET /api/community?part=reviews&code=<CODE>` already returns `avg` and `count` of approved reviews. Show
  "4.6 (23)" on the card and subject page when count is 3 or more.

## Files
`api/session.js`, `src/scripts/core.js` or `home.js` (cards), `src/pages/[code].astro`, `src/components/Price.astro`.

## Acceptance
With fake data of 12 buyers and 4 approved reviews the card shows both; with 5 buyers and 1 review it shows
neither. No email appears in any public answer. `npm test`, `npm run build`.

## Done when
Visible locally (`npm run dev:local`), committed.
