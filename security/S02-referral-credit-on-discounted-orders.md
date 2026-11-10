Status: TODO

# S02 Referral cash can be more than the friend paid (High)

> **OWNER APPROVAL REQUIRED: touches payments/referrals/admin/access.** Show the diff, wait for a yes, then commit. No push.

## Problem
A friend's first purchase credits the referrer Rs 3 even when a discount code cut the price to Rs 1-2. Colluding
accounts then earn more than they spend.

## Evidence
- `vit-notes-store/api/create-order.js:57-66`: the code branch runs, then `ref` is still written into the notes.
- `vit-notes-store/api/_lib.js:167`: `orderInfo` keeps `ref` whenever the order is not a gift.
- `vit-notes-store/api/_referrals.js:32`: `CASH = 300`. `_offers.js:33` allows codes up to 99% off and `:44`
  floors the price at Rs 1.

## Fix (owner picks; A is the smallest)
A. No credit when a code was used: in `create-order.js` skip `refs.eligible` when `offer` is set; in
   `_lib.js recordPurchase` set `firstBuy = false` when `chk.offer` is set (covers orders already created).
B. A floor: in `_referrals.credit` return false unless `payment.amount >= 1000` paise, and never pay more than
   20% of the amount paid.
Doing both is safest: A stops stacking, B covers future cheap sales.

## Files
`vit-notes-store/api/create-order.js`, `api/_lib.js`, `api/_referrals.js`, `tests/referrals.test.js`.

## Prove it
Add to `tests/referrals.test.js`: (1) order with a 90% code and a valid ref, payment verified: referrer's `friends`
stays 0; (2) full-price order with ref: `friends` is 1; (3) amount under the floor: no credit.
Run `npm test` and `npm run build` in vit-notes-store.

## Rollback
`git revert`. Credits already recorded are not touched either way.

## Done when
Owner chose A, B or both; tests added and green; committed locally.
