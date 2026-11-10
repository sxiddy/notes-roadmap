Status: TODO (one status per subject below)

# Content review runbooks (use with NOTES-REVIEW-BRIEF.md in this folder)

Every review session: read `HOW-TO-USE.md`, then `NOTES-REVIEW-BRIEF.md` fully, then only the section for the
subject. One module per session, one local commit per module, no push. Coverage script and `validate.js` decide
what to fix; a model does not re-read passing notes. Open the page image before "fixing" a figure-only fact
(lesson 31).

## English (BAENG101) — Status: TODO — do this FIRST: content is unfinished
Folder `baeng101-notes` (main). Read `src/RESUME.md` (local, gitignored: if missing, the list below is its content).
1. Missing question files named in `content/modules.json`: `q-M1.js`, `q-M2.js`, `q-M3b.js`, `q-M4b.js`,
   `q-M5c.js`. Per `src/drive-text/BRIEF.md`: at least 5 MCQs and 2-3 open questions per topic, scenario style.
   One file per session. Note: `protected/data` already holds some of these names; compare with `content/` first.
2. Summaries: new `summary-m1.js`, `summary-m2.js`; extend `summary-m3/m4/m5.js` with the new topics.
3. Run `node tools/build.js` and `node tools/qa.js` (the chunk agents never ran them); look at the new SVG figures
   in the real app.
4. Double-check the derived answers listed in RESUME.md (m1 exercises, m3 inferencing Q8, m4 review types, m5 case
   studies) against the Drive text; APA 6th vs 7th edition note; chunk-O source labels.
5. Papers data: transcription was started and not finished; decide with the owner whether to finish.
6. Then S09 (port to the shared shell), then the normal review below.
Never copy the personal email address that appears on a Module 5 practice sheet.

## Chemistry (BACHY105) — Status: TODO
Folder `bachy105-notes` (main). `tools/qa.js` cannot run on this PC (missing package, lesson 28): run
`npm install` in `tools/` only if the owner agrees, else rely on `validate.js`. `tools/build.js` rewrites 6
stale generated files: commit only what you changed. No per-module coverage script here: use
`notes-tools/coverage.js` once built, or copy `baeee101-notes/tools/check-m1.js` as a pattern. Past papers
exist (`papers-free`, `papers-pro-answers`): weight topics the papers stress.

## Basic Engineering (BAEEE101) — Status: TODO
Folder `baeee101-notes` (main; has an uncommitted change in `api/_battle.json`: ask before touching). Coverage
scripts exist: `tools/check-m1.js` .. `check-m4.js`, `check-papers.js`. An earlier audit committed M1 and M2 fixes
and was applying M3 (dimension rules, quadrants, section views, worked problems); M4 audit and questions for
3c, 3d, 4e, 4f were still open (evs-notes PLAN.md line 197). Check `git log` to see where it stopped.

## EVS (BACHY101) — Status: TODO
Folder `evs-notes` (master). Read `PLAN.md` "State" only. Weeks 1-5 only; never build Week 6. On 11-11-2026
`validate.js --all` gave 0 errors and 8 warnings: two uses of the banned word "landscape" in `chunk-D.js`
(m2-conservation-levels) and six thin tabs (chunk-E m2-gm-flowers "India", chunk-H m3-water-basics "WHO
standards", chunk-K m4-eia-basics "Objectives" and "Principles", m4-eia-methods "Comparison", chunk-M
m4-water-act-basics "Seven needs"). Fix those first. MCQ free-model check stopped at 152 of 269 (0
disagreements); finishing it is optional. No past papers exist for EVS.

## Done when
Each subject's section says DONE with a date, and every module has a commit "review: <module>".
