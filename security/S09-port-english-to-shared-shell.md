Status: TODO

# S09 Technical English runs an older shell than the other three sites (Medium)

> **OWNER APPROVAL REQUIRED: touches payments/referrals/admin/access.** Show the diff, wait for a yes, then commit. No push.

## Problem
baeng101-notes was built before the Pro split. Its api and app differ from the other sites, so every shared fix
must be hand-ported, and Pro there unlocks only five interactive models for the same price.

## Evidence (`baeng101-notes`)
- `api/content.js:10`: file pattern has no `pro` or `qa` (no flashcard or answer split).
- `api/_access.json`: no `proFiles`; `papers-paid.js` is in `freeFiles`; Pro = `viz-*.js` only.
- `api/_lib.js:109-117,221-237`: own switches (`signin` "1"/"0", `access`) instead of `siteMode()` (`signin`
  off, `mode`). `api/session.js:33` returns `open`/`freeAll`.
- Admin page is `app/stats.html`; `vercel.json` redirects `/admin` there. No `admin.html`.
- Content is unfinished: see `baeng101-notes/src/RESUME.md` and `content-review/RUNBOOK-english.md`.

## Fix (after the content in RESUME.md is finished, or it is ported twice)
1. Copy from baeee101-notes: `api/*.js`, `app/` shell (`index.html`, `js/app.js`, `gate.js`, `boot.js`,
   `data-files.js`, `css/app.css`, `admin.html`), `tools/*`, `vercel.json`. Keep baeng101's `content/`, `src/`,
   `viz-paid/`, `figs-svg/`, `app/js/viz/`.
2. Run `baeee101-notes/tools/rebrand.js` (read it first) or rename by hand: code BAENG101, name Technical English,
   product tag `baeng101-notes`, `STORE_CODE`, `REVIEW_EMAIL`, prefix `baeng101:`, `storeName` in `session.js`.
   Then grep for Basic Engineering topic words (lesson 5 in NOTES-SITES-LESSONS.md).
3. Convert question files to the split format the build expects (questions in `q-*.js`, answers in `qa-*.js`).
4. Redis: map old config fields to new ones once (`signin` "0" to `off`; `access` to `mode`). Write a tiny script;
   the owner confirms on `/admin` afterwards.
5. Delete generated `api/_access.json`, `_battle.json`, `_content.json`; rebuild.

## Prove it
`node tools/validate.js --all` 0 errors, `node tools/build.js`, `node tools/qa.js` and `qa.js --free` clean.
`diff --strip-trailing-cr` of each `api/*.js` against baeee101 shows only the subject names.

## Rollback
Work on a branch `shared-shell`; main is untouched until the owner says push.

## Done when
Branch passes all checks, owner reviewed the Pro contents list, merged locally.
