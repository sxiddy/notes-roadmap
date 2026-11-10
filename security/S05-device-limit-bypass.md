Status: TODO

# S05 A sign-in without a device id is never limited (Medium)

> **OWNER APPROVAL REQUIRED: touches payments/referrals/admin/access.** Show the diff, wait for a yes, then commit. No push.

## Problem
The 1 phone + 1 PC limit depends on the browser sending a device id. A cookie issued without one passes every check,
so one account can be shared widely by anyone who blocks that id.

## Evidence (same code in all four sites; evs-notes line numbers)
- `api/login.js:35-40`: no valid id gives `userCookie(email, null)`.
- `api/_lib.js:310`: `if (!u || !u.d) return true;`
- `api/session.js:20-23`: the slot is claimed only if the page later calls `/api/session?d=<id>`.

## Fix
1. `_lib.js deviceActive`: a cookie without `d` is accepted only for 10 minutes after its `t` (sign time). After
   that return false, so `hasAccess` and `/api/sync` refuse and `session.js` shows the sign-in screen.
2. Leave the claim path in `session.js` alone: it re-issues the cookie with `d`, so normal students see nothing.
3. Optional: refresh the ZADD score in `session.js` so "least recently used" is really by last use.

## Files
`api/_lib.js`, `api/session.js` in all four repos (baeng101 by hand).

## Prove it
With `tools/fakeredis.js`: a cookie without `d` and `t` 11 minutes old gets no access (`hasAccess` null, Pro file
401/402); a fresh one is claimed by `/api/session?d=<32 hex chars>`. Then `node tools/qa.js` (when its port is free).

## Rollback
`git revert`. Reverting locks nobody out.

## Done when
Test passes on one site, the change is copied to the other three, each committed.
