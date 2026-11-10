Status: TODO

# S01 Sites must default to paid and signed-in (High)

> **OWNER APPROVAL REQUIRED: touches payments/referrals/admin/access.** Show the diff, wait for a yes, then commit. No push.

## Problem
If a site's Redis hash `config` is lost or emptied, three sites fall back to "Pro free", and two of them also to
"sign-in off", while the store keeps charging for Pro.

## Evidence (11-11-2026)
- `baeee101-notes/api/_mode.json` and `baeng101-notes/api/_mode.json`: `"open": true`.
- `baeee101-notes/api/_lib.js:93-107`: `open` seeds `signin: !open`, `freeMode: open`; Redis only overrides when set.
- `baeng101-notes/api/_lib.js:109,221-228`: same idea through `settings()` (`signin`, `freeAll`).
- `bachy105-notes/api/_lib.js:101`: `out.freeMode = m !== 'paid'` (free unless the field says paid).
- `evs-notes/api/_lib.js:101` is correct: `out.freeMode = m === 'free'`.
- Live check: all four sites answered sign-in on and not free, so the switches are set today. The risk is latent.

## Fix
1. bachy105: change line 101 to `m === 'free'`; copy the comment at lines 89-90 from evs-notes.
2. baeee101 and baeng101: set `"open": false` in `api/_mode.json`.
3. BEFORE deploying each, open the site's `/admin` and confirm it shows paid and sign-in on, so nothing changes for
   students. A site the owner wants free is switched on `/admin` (writes `mode=free`).
4. Owner checks that `OPEN_ACCESS` is not set in Vercel for baeee101 and baeng101.

## Files
`bachy105-notes/api/_lib.js`, `baeee101-notes/api/_mode.json`, `baeng101-notes/api/_mode.json`.

## Prove it
- No Redis env, in baeee101: `node -e "require('./api/_lib').siteMode().then(console.log)"` prints `signin: true`,
  `free: false`. In baeng101 call `settings()` and expect `signin: true`, `freeAll: false`.
- bachy105 with `tools/fakeredis.js` and an empty `config`: `siteMode()` gives `free: false`.
- After the owner's push: `curl https://<site>/api/session` shows sign-in on and not free; an anonymous
  `curl -o /dev/null -w "%{http_code}" https://<site>/data/<a Pro file>` gives 401
  (bachy105 `pro-A.js`, baeee101 `pro-1a.js`, baeng101 `viz-m4.js`).

## Rollback
`git revert` in each repo and redeploy. The `/admin` switches work either way.

## Done when
Three repos committed, local checks pass, and after "push" the live checks pass.
