Status: TODO

# S06 Store and Chemistry write unprefixed keys into the shared Redis (Medium)

> **OWNER APPROVAL REQUIRED: touches payments/referrals/admin/access.** Show the diff, wait for a yes, then commit. No push.

## Problem
All sites share one Upstash database. Three sites prefix their keys; the store and bachy105 do not, so keys with
the same name mix and any new key name can collide.

## Evidence
- `bachy105-notes/api/_store.js:12-22`: no prefix. Others: `evs-notes/api/_store.js:14` (`bachy101:`), baeee101
  (`baeee101:`), baeng101 (`baeng101:`).
- `vit-notes-store/api/_store.js`: no prefix; `vit-notes-store/api/track.js:1` says the database is shared.
- Same names on both sides: `userFirst`, `userLogin`, `userLogins` (store `api/login.js:20`, site `api/login.js:39`)
  and `revtry` (reviewer limit). NOT CONFIRMED: that the store and bachy105 really use the same database (owner
  checks the Vercel storage connections).

## Fix (a data migration: do it in a quiet hour, payments paused on `/admin`)
1. Prefixes: `bachy105:` and `store:`.
2. Write `tools/migrate-prefix.js`: `SCAN` every key without a known prefix and write the list to a file (dry
   run). The owner reads the list. Then `RENAME key <prefix>key`. Store keys by name: `paid:*`, `refunded:*`,
   `refunds`, `ref:*`, `offers`, `offer-*`, `ann`, `req*`, `rev`, `rep`, `storeVisits`, `alert*`, `cronlock:*`.
   Everything else unprefixed is bachy105's. Copy the three mixed hashes to both. Build a `--reverse` flag.
3. bachy105: copy `_store.js` from baeee101 with the new prefix. Store: add the same `withPrefix` wrapper.
4. Deploy and migrate together.

## Prove it
Before: run the weekly backup (`/api/cron?job=weekly` with the header) and note the store admin totals and the
bachy105 `/admin` student count. After: `DBSIZE` unchanged and both numbers match.

## Rollback
`node tools/migrate-prefix.js --reverse`, then redeploy the previous commits.

## Done when
Both apps prefixed, numbers match, backup kept, owner signed off.
