Status: TODO

# C8 The invited friend also gets something (two-sided referral)
Value medium · Cost medium (about 6%) · Payment code: YES · Order: 8 · Depends on: S02, S03

> **OWNER APPROVAL REQUIRED: touches payments/referrals/admin/access.** Show the diff, wait for a yes, then commit. No push.

## Buyer benefit
Today only the inviter gains, so the friend has no reason to use the link. Rs 2 off for the friend fixes that.

## Design
- In `api/create-order.js`, when `refs.eligible(b.ref, u.e)` returns a token (first purchase, not a gift, not
  self), take `FRIEND_OFF` (200 paise) off the order total, never below Rs 1 per subject, and write `shares` and
  `notes.offer = 'FRIEND'` like a code. No discount code on the same order.
- Keep the S02 floor: with the Rs 20 sale price the friend pays Rs 18 and the inviter earns Rs 3; the store keeps
  Rs 15 before fees. Recheck the numbers if the price changes (`CASH` in `api/_referrals.js:32`).
- Page: when `?ref=` is present show "Your friend's link: Rs 2 off your first subject".

## Files
`api/create-order.js`, `api/_lib.js` (skip `_offers.spend` for `FRIEND`), `src/scripts/core.js`,
`tests/referrals.test.js`.

## Acceptance
Tests: first purchase with a valid link costs Rs 2 less and still credits the inviter; second purchase gets no
discount; own link gets none; gift gets none; code + link refused. `npm test`.

## Done when
Owner approved the amounts, tests green, committed.
