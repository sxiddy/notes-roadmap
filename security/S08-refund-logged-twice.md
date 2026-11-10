Status: TODO

# S08 A partial refund of a cart payment is logged up to three times (Medium)

> **OWNER APPROVAL REQUIRED: touches payments/referrals/admin/access.** Show the diff, wait for a yes, then commit. No push.

## Problem
The owner refunds one subject of a two-subject cart from the admin page. The admin action logs the refund once.
Razorpay then sends a refund webhook, and the webhook logs it again once per subject in the cart, each with the full
refund amount. "Refunded" totals on the admin pages and in the weekly summary are too high. Pro access is not affected.

## Evidence (`vit-notes-store`)
- `api/admin.js:171-172`: admin refund calls `recordRefund` with `refundId: rf.id`.
- `api/verify.js:54-55`: the webhook loops over every item with `refundId: rf.id + '_' + code` and `amount: rf.amount`.
- `api/_lib.js:252`: `HSETNX refunds <refundId>` only de-duplicates identical ids.
- Applies only when `RAZORPAY_WEBHOOK_SECRET` is set (owner confirms).

## Fix
1. In the webhook branch, before logging, read hash `refunds` and skip when any entry already has the same
   Razorpay refund id as its id or as the part before `_`.
2. For a partial refund of a cart made in the Razorpay dashboard (no admin entry), log ONE entry with
   `product: 'cart'` and the real amount, not one per subject.
3. Full refunds keep the per-subject loop (each subject's record must move to `refunded:<CODE>`), but split the
   amount by each record's own `amount` (its share) instead of repeating `rf.amount`.

## Files
`vit-notes-store/api/verify.js`, `api/_lib.js` (small helper), `tests/` (new `refunds.test.js`).

## Prove it
Tests with fake Razorpay answers: (1) admin partial refund then webhook: hash `refunds` has one entry and the
total equals the refund; (2) dashboard full refund of a two-subject cart: two entries whose amounts add up to the
payment, both subjects moved to `refunded:*`. `npm test`.

## Rollback
`git revert`. Old duplicate entries stay; list them for the owner to delete by hand if wanted.

## Done when
Tests green and committed.
