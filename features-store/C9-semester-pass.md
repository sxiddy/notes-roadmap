Status: TODO

# C9 Semester pass (every subject, including ones added later)
Value high once there are 8+ subjects · Cost large (about 12%) · Payment code: YES · Order: 9 (after C7 proves demand)

> **OWNER APPROVAL REQUIRED: touches payments/referrals/admin/access.** Show the diff, wait for a yes, then commit. No push.

## Buyer benefit
Pay once, get every subject for the semester. For the owner: money up front and a reason to keep adding subjects.

## Design
- New product code `PASS` that is not a subject: record `paid:PASS` with `until` = end of semester (the `until`
  field already ends manual grants: `isLive`, `api/_lib.js:220`).
- `ownedBy` (`api/_lib.js:237-245`) and `api/entitlement.js:49-50`: a live `paid:PASS` record counts as owning every
  subject. Sites need no change (they ask the store).
- `PASS` must not be a key of `PRODUCTS` (that map drives subject pages, cron site checks and admin loops). Keep it in
  a separate `PASSES` map and extend `itemsOf`/`isProduct` carefully; read every use of `PRODUCTS` first.
- Refund of a pass ends it for all subjects. Referral credit applies once, under the S02 rules.
- Price and end date are owner decisions; put them in one constant block next to `SALE`.

## Files
`api/_lib.js`, `api/create-order.js`, `api/entitlement.js`, `api/admin.js` (show passes), `api/session.js`,
`src/pages/index.astro`, `src/scripts/core.js`, `tests/pass.test.js`.

## Acceptance
Tests: pass buyer is entitled to every code including one added after purchase; after `until` not entitled;
refund ends it; a pass holder cannot be charged for a single subject (409). `npm test`.

## Done when
Owner set price and dates, tests green, committed.
