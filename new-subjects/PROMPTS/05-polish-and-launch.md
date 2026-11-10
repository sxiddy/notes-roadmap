# Prompt 05: Polish, figures, launch

## 05a Polish (strongest model, one module per session)
```
Subject [CODE], module [m]. Read BRIEF.md, STATE.md and the flagged list for this module only
(validate warnings, coverage gaps, MCQ disagreements). Fix only those items, opening the page image in
src/pages/ when a fact is in a figure. Do not rewrite passing notes.
Run node tools/validate.js --all, node ../notes-tools/coverage.js . --all, node tools/build.js, then
node tools/qa.js and node tools/qa.js --free (only if no other session is using port 4183).
Run git status and add only the files you changed. Commit. Update STATE.md.
```

## 05b Figures, Lite tier (script first)
```
Subject [CODE]. Run node tools/fig.js content/chunk-[X].js for each chunk (crop is [x, y, width, height]).
Tile tmp/preview crops into one contact sheet per module and look at each sheet once. Inset any crop that
caught page text by about 1.5%. Rebuild, commit.
```

## 05c Figures, Full tier
```
Follow tools/figs/PLAYBOOK.md exactly: one batch of 10-14 figures per session, one generator file per batch,
one preview per seven figures, stop at 85%. Commit locally; never push.
```

## 05d Launch
```
Subject [CODE]. Work through CHECKLIST.md "Infrastructure" and "Store listing" top to bottom.
Stop and ask me at every line marked OWNER. Never print env values: copy them between Vercel projects by script.
Do not push until I say "push". After deploy run node ../notes-tools/launch-check.js https://[site] [CODE]
and show me the PASS/FAIL lines.
```
