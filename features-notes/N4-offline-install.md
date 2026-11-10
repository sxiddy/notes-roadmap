Status: TODO

# N4 Installable app that opens the free notes offline
Value medium · Cost medium (about 8%) · Money/accounts: reads sign-in state only · Order: 5 · Depends on: S05, S13 decided

## Goal and benefit
"Add to home screen" and notes that open in the hostel with no network.

## Design
- `app/manifest.webmanifest` (name, icons, theme colour, `display: standalone`) and `app/sw.js`.
- Service worker caches the shell (`index.html`, `js/*`, `css/app.css`) and, after a successful fetch, the FREE data
  files and figures only (`/data/<free file>`, `/img/...`). Never cache Pro files (`pro-`, `qa-`, `viz-`,
  `papers-pro`): the list of free files is already in `app/js/data-files.js`.
- Cache per signed-in email; clear it on sign-out and on a 401 from `/api/session` (device signed out).
- Stale-while-revalidate, with the build writing a version string so a new deploy replaces old files.

## Files
`app/manifest.webmanifest`, `app/sw.js`, `app/index.html` (link + register), `app/js/boot.js`, `tools/build.js`
(version string), icons in `app/`.

## Acceptance
Chrome DevTools offline: a visited free topic opens with its figures; a Pro file request is not served from cache;
after sign-out the cache is empty. Lighthouse "installable" passes. `node tools/qa.js` clean.

## Risk
A cached shell can hide a fix: test the update path twice before pushing.

## Done when
Works on a real phone (owner), committed.
