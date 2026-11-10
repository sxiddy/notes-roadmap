Status: TODO

# S11 Stale text left from earlier versions (Low)

## Problem
Three strings describe an older version of the sites and mislead students or search results.

## Evidence
- `evs-notes/app/index.html:7` and `bachy105-notes/app/index.html:7`: meta description says "modules 2–4"; both
  sites have five modules.
- `api/battle.js:39` in all four sites: "Free accounts get one preview battle on a single Module 2 topic." Every
  module is free now (`api/_access.json` `freeModules` lists all of them), so any single topic works.
- `evs-notes/api/_store.js:12-13`: comment still describes the prefix as shared "with the chemistry site" (fine, but
  reword so nobody thinks EVS reuses chemistry keys).
- Default prices in code differ and are all stale: `PRICE_PAISE` fallback in `api/_lib.js:4` is 2000 (evs), 5000
  (bachy105), 1000 (baeee101, baeng101). The store price replaces them when the store answers.

- `evs-notes/app/js/notix-extras.js:7`: `FREE_TOOL = 'beer-lambert'`, a chemistry tool id copied with the shell.
  Check whether EVS has a tool with that id; if not, set it to an EVS model id (see `viz-paid/`) or remove the label.

## Fix
1. Rewrite the two meta descriptions without module numbers.
2. battle.js: "Free accounts get one preview battle on a single topic."
3. Set every `PRICE_PAISE` fallback to the store's list price (3000 on 11-11-2026; check
   `vit-notes-store/api/_lib.js:15-18` first) so an outage never shows Rs 50.
4. Check `app/index.html` is the source (not rewritten by `tools/build.js`) before editing; if the build writes it,
   edit the template the build reads.

## Files
`app/index.html` (evs, bachy105), `api/battle.js` and `api/_lib.js` (four sites).

## Prove it
`grep -rn "modules 2" app/ api/` and `grep -n "Module 2 topic" api/battle.js` return nothing in each site;
`node tools/build.js` leaves the edits in place; `git status` shows only intended files.

## Rollback
`git revert`.

## Done when
Greps clean in all four repos, committed.
