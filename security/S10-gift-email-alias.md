Status: TODO

# S10 A gift can be sent to an address nobody can sign in with (Low)

> **OWNER APPROVAL REQUIRED: touches payments/referrals/admin/access.** Show the diff, wait for a yes, then commit. No push.

## Problem
The gift field accepts any text that looks like a student email, including `name+tag@...` or a typo. Pro is then
recorded under an address Google never returns at sign-in: the buyer paid and nobody has Pro.

## Evidence
- `vit-notes-store/api/_lib.js:89`: `isStudentEmail` allows `+`, `%` and any local part.
- `vit-notes-store/api/create-order.js:29-33`: the gift email is only checked with that function.

## Fix
1. `create-order.js`: for a gift, refuse `+` and `%` in the local part (400 with `badGift: true`: "Use your
   friend's exact VIT email").
2. If hash `userFirst` has no entry for the friend, still allow it but return `giftUnknown: true`, and have the
   page (`src/scripts/core.js`) ask "We have not seen this email sign in yet. Is it spelled right?" before paying.
3. Admin: add a "move Pro to another email" action in `api/admin.js` (revoke + grant keeping the payment record)
   so a typo can be fixed without a refund.

## Files
`vit-notes-store/api/create-order.js`, `api/admin.js`, `src/scripts/core.js`, `tests/gifts.test.js`.

## Prove it
Tests in `tests/gifts.test.js`: `a+b@<domain>` is refused; an unseen email returns `giftUnknown`; "move" keeps
the payment id and amount under the new email. `npm test`.

## Rollback
`git revert`.

## Done when
Tests green, committed.
