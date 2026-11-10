Status: TODO

# N7 Weekly battle leaderboard
Value medium · Cost medium (about 6%) · Money/accounts: accounts (shows first names) · Order: 7

> **OWNER APPROVAL REQUIRED: touches accounts (shows student first names to other students).**

## Goal and benefit
"Top 10 this week" on the battle page. Competition brings students back and gives free users a reason to go Pro
(free accounts get one preview battle).

## Design
- When a battle finishes, add each player's points to a sorted set `lb:<ISO week>` (`ZINCRBY`), member = email,
  `EXPIRE` 21 days. Scores are computed in `view()` in `api/battle.js:67-91`; add a one-time write guarded by
  `HSETNX bt:<id> scored 1` so polling does not count twice.
- `GET /api/battle?board=1` returns the top 10 as `{ name: firstName(email), points }` plus the caller's rank.
  Never return emails (`firstName` is at `api/battle.js:23`).
- Vercel Hobby allows 12 functions and the sites are at the limit (`api/sync.js:6`): fold this into `battle.js`.
- Opt-out switch stored in the synced progress object; opted-out players are skipped when writing.

## Files
`api/battle.js`, `app/js/app.js` (battle page), `app/css/app.css`.

## Acceptance
Two test accounts finish a battle (fakeredis): both appear once with the right points; polling the finished battle
again does not change them; an opted-out player is absent; the response contains no `@`.

## Done when
Tested locally on one site, copied to the rest, owner approved name display, committed.
