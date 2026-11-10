Status: TODO

# S13 Raw server errors reach students; sessions never expire (Low)

> **OWNER APPROVAL REQUIRED: touches payments/referrals/admin/access.** Show the diff, wait for a yes, then commit. No push.

## Problem
1. Any thrown error is sent to the browser as is: missing-variable messages name the env var and say where to set
   it, and Razorpay's own error text is passed through.
2. Sign-in cookies last 400 days and are never checked for age on the server, so a copied cookie works for over a
   year (the device check is the only brake, see S05).

## Evidence (evs-notes line numbers; same in every site and in the store's `api/_lib.js`)
- `api/_lib.js:14-18` `env()`: "Server is missing <NAME>. Set it in Vercel ...".
- `api/_lib.js:190`: Razorpay `error.description` becomes the message.
- `api/_lib.js:318-323` `handle()`: `send(res, status, { error: e.message })`.
- `api/_lib.js:8,47,50-59`: `MAX_AGE` 400 days; `t` is written into the cookie but `readSigned` never reads it.

## Fix
1. `handle()`: for status 500 and above, `console.error` the real message and send
   "Something went wrong on our side. Please try again." Keep 4xx messages (they are written for students).
   The store already alerts the owner by Telegram for 5xx (`vit-notes-store/api/_lib.js:269`); keep that.
2. `readSigned`: return null when `t` is older than 180 days. In `session.js`, re-issue the cookie when `t` is
   older than 7 days, so active students are never signed out.
3. Owner cookie (`bn_access`): expire after 30 days.

## Files
`api/_lib.js`, `api/session.js` in four sites; `api/_lib.js`, `api/session.js` in the store.

## Prove it
Unset `SESSION_SECRET` locally and call `/api/login`: the answer has the generic text and the log has the detail.
A cookie signed with `t` 181 days back is refused; one 8 days old gets a fresh `Set-Cookie` from `/api/session`.

## Rollback
`git revert`.

## Done when
Both checks pass on one site and the store; copied to the rest; committed.
