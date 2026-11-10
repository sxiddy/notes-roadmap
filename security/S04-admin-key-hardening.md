Status: TODO

# S04 One admin key for five sites, accepted in a URL, no guess limit (Medium)

> **OWNER APPROVAL REQUIRED: touches payments/referrals/admin/access.** Show the diff, wait for a yes, then commit. No push.

## Problem
The same `ADMIN_KEY` unlocks the store and every notes site. The cron endpoint also accepts it as a query
parameter (URLs land in logs and history). No admin endpoint limits wrong guesses.

## Evidence
- `vit-notes-store/api/admin.js:21` and `api/cron.js:49`: the store sends `ADMIN_KEY` and `STORE_SECRET` to each site.
- `vit-notes-store/api/cron.js:17`: `searchParams.get('key')` is accepted.
- No limit on failures: store `api/admin.js:122-124`, `api/offers.js:23-27`, `api/community.js:76-80`; every site's
  `api/stats.js:78-79`.

## Fix
1. `cron.js`: remove the `?key=` form (keep the header); fix the comment at line 4.
2. Add `adminGuard(req)` to each `_lib.js`: on a wrong key `INCR adminfail:<ipHash>` with `EXPIRE 900`; answer 429
   after 10 failures. Use it in every check listed above.
3. Let sites accept `Authorization: Bearer <STORE_SECRET>` for the read-only `GET /api/stats`, and stop sending
   `ADMIN_KEY` from the store. POST actions still need the site's own key.
4. Owner then sets a different `ADMIN_KEY` per Vercel project (owner step; values never in chat or files).

## Files
Store: `api/cron.js`, `api/admin.js`, `api/offers.js`, `api/community.js`, `api/_lib.js`.
Sites (four repos): `api/stats.js`, `api/_lib.js`. baeng101's files differ: patch by hand.

## Prove it
- Store test: 11 wrong keys from one address give 429; a right key works after the window.
- After deploy: the cron URL with `&key=anything` gives 401.
- Store admin "Visits" and "People" tabs still show numbers from every site.

## Rollback
`git revert` per repo. Deploy sites before the store, or the store cannot read their stats.

## Done when
Sites then store deployed, the three checks pass, owner rotated the keys.
