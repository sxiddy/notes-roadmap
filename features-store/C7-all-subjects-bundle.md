Status: TODO

# C7 All-subjects bundle price
Value high · Cost medium (about 8%) · Payment code: YES · Order: 7

> **OWNER APPROVAL REQUIRED: touches payments/referrals/admin/access.** Show the diff, wait for a yes, then commit. No push.

## Buyer benefit
One tap, every subject, cheaper than buying one by one. Raises the average order.

## Design
- The cart already takes several subjects in one order (`api/create-order.js:26-27`, `itemsOf` allows up to 10)
  and records each subject's share (`recordPaidAll`, `api/_lib.js:179-186`).
- Add a bundle rule in `create-order.js`: when the cart holds every subject the buyer does not own (3 or more),
  apply `BUNDLE_PCT` (owner picks, e.g. 25) through `offers.priceCart(items, priceOf, { pct, codes: [] })` and
  write `shares` into the notes exactly as a discount code does (`create-order.js:60-62`), with
  `notes.offer = 'BUNDLE'`.
- Guard `_offers.spend`: skip it when the offer is `BUNDLE` (not a real code) in `recordPurchase`
  (`api/_lib.js:215`).
- One discount per order: bundle and code together are refused, like reward and code.
- `GET /api/session` returns `bundle: { pct, total, list }` so the page can show the button. Raise the 10-item
  cap in `itemsOf` before there are more than 10 subjects.

## Files
`api/create-order.js`, `api/_lib.js`, `api/session.js`, `src/scripts/core.js`, `tests/` (new `bundle.test.js`).

## Acceptance
Tests: bundle of 4 at 25% gives the right total and shares that add up; each subject is recorded with its share;
a refund of one subject refunds only its share; bundle + code is refused; S02 rule still holds. `npm test`.

## Done when
Owner chose the percent, tests green, committed.
