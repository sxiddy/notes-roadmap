Status: TODO

# C4 Sale countdown and exam-date urgency
Value medium · Cost small (about 2%) · Payment code: not touched (reads the sale end only) · Order: 4

## Buyer benefit
An honest reason to buy now: the launch price ends on a fixed date, and the exam is N days away.

## Design
- The sale end is fixed in code: `api/_lib.js:23` (`SALE.until`), and `src/components/SaleNote.astro` already
  shows a sale note. Check whether `/api/session` exposes the end time; if not add `saleUntil` (a number, public).
- Show "Launch price ends in 12 days" (days only; a ticking clock only in the last 48 hours). Hide it by itself
  after the end. Never show a countdown that restarts.
- Exam line: a small data file `src/data/exams.mjs` (`{ CAT1: '2026-..', FAT: '..' }`) the owner fills in; show
  "CAT-2 in 9 days" on subject pages. Empty file: nothing shown.

## Files
`api/session.js` (one field), `src/components/SaleNote.astro`, `src/scripts/core.js`, `src/data/exams.mjs`.

## Owner input
Exam dates.

## Acceptance
With the clock set after the sale end the note is gone and prices show the list price. `npm test` (tests pin
`SALE_PCT`, see lesson 21), `npm run build`.

## Done when
Committed; owner supplied dates or left the file empty on purpose.
