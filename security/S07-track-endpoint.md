Status: TODO

# S07 /api/track accepts anything from anyone (Medium)

## Problem
No sign-in and no rate limit: anyone can inflate visit and quiz numbers and add unlimited fields to the `quiz`,
`quizLabel`, `confusing` and `confusingLabel` hashes (the free Redis plan has a size cap).

## Evidence
`api/track.js:14-34` in all four sites (evs-notes line numbers). `QID` at line 11 accepts any id of 1-40 characters.

## Fix
1. Rate limit per address: `INCR rl:tr:<ipHash>:<hour>` with `EXPIRE 3700`. Over 600 an hour: answer 200 and store
   nothing.
2. Accept only real ids: load topic and question ids from `api/_content.json` (written by `tools/admin-index.js`
   on every build) into a Set; ignore unknown `quiz`, `topic` and `confusing` values. If the index has no question
   ids yet, extend `tools/admin-index.js` to write them.
3. Take labels from that index, not from the request (`b.label`).

## Files
`api/track.js`, `tools/admin-index.js` in each site.

## Prove it
With fakeredis: a POST with an unknown quiz id adds nothing, a real id adds 1, and the 601st call in an hour adds
nothing. `node tools/build.js` then `node tools/qa.js` report no errors; the `/admin` content section still shows labels.

## Rollback
`git revert`.

## Done when
Checks pass on one site and the change is copied to the other three.
