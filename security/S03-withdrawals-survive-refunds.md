Status: TODO

# S03 A refund does not claw back referral cash already requested (High)

> **OWNER APPROVAL REQUIRED: touches payments/referrals/admin/access.** Show the diff, wait for a yes, then commit. No push.

## Problem
Five friends buy, the referrer withdraws Rs 15, the friends are refunded. The friends leave the count, but the
pending or paid withdrawal stays and the balance is clamped at 0, so the debt disappears.

## Evidence (`vit-notes-store/api/_referrals.js`)
- `:97-106` `onRefund`: only marks `ref:by` revoked and deletes the friend.
- `:119`: `balance: Math.max(0, earned - withdrawn)`.
- `:142-151` `markPaid`: pays whatever the queue says, with no recheck.
- `:153-169` `adminList`: `pending` carries no current earned total.

## Fix
1. `rewardsOf`: also return `owed = Math.max(0, withdrawn - earned)`.
2. `adminList`: add `earnedNow` and `overdrawn` to each pending withdrawal; show a red "refunded friend: do not
   pay" in `public/js/admin-referrals.js`.
3. `markPaid(id)`: answer 409 when overdrawn unless the request has `force: true`.
4. Hold period: a friend counts toward withdrawable cash 7 days after their payment (the `at` already stored in
   `ref:friends:<ref>`). Show "Rs X available on <date>" in the invite panel (`src/scripts/core.js`).
5. New admin action `ref-cancel { id }` in `api/offers.js`: removes the entry from `ref:wdq` and `ref:wd:<email>`.

## Files
`api/_referrals.js`, `api/offers.js`, `public/js/admin-referrals.js`, `src/scripts/core.js`, `tests/referrals.test.js`.

## Prove it
Tests: (1) 5 credits, withdraw, refund one friend: admin list marks it overdrawn and `ref-paid` answers 409;
(2) credits younger than 7 days cannot be withdrawn; (3) `ref-cancel` restores the balance. `npm test`.
Then `npm run dev:local` and look at the admin referrals tab.

## Rollback
`git revert`. No key changes shape, so there is no migration.

## Done when
Tests green, warning visible locally, owner approved the 7-day hold, committed.
