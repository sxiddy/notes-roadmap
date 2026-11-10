Status: TODO

# N5 One-page cram sheet per module (Pro)
Value medium · Cost small (about 3% for the feature; content is script-built) · Money/accounts: uses existing Pro gate · Order: 4

## Goal and benefit
A single printable page per module for the last hour before the exam: the one-liners of every topic plus the
module's formulas or key numbers.

## Design
- No new writing: each topic already has `oneLiner` entries in `content/chunk-*.js`, and summaries have "big
  ideas" (`content/summary-m*.js`). `tools/build.js` collects them into `protected/data/pro-cram-<module>.js`
  (name must match the pattern in `api/content.js:10`: `pro-[A-Za-z0-9-]+`), listed in `proFiles`.
- The app renders it as a two-column print layout and reuses the existing PDF-save path (check how the revision
  pack prints first; do not build a second one). If the revision pack already covers this, close the item as
  "duplicate" instead.

## Files
`tools/build.js`, `app/js/app.js`, `app/css/app.css` (print rules).

## Acceptance
Module 1 sheet fits on one A4 page at default print scale; non-Pro users get the lock card; the file returns 402
for a signed-in non-Pro user. `node tools/validate.js --all`, `build.js`, `qa.js` clean.

## Done when
Built for every module of one site, copied to the others, committed.
