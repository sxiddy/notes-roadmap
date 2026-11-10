Status: TODO (build the scripts in SCRIPTS-TO-BUILD.md once, then run this per subject)

# Pipeline for a new subject

Baseline (measured): a subject built the old way costs about 1.3-1.7M tokens; EVS Module 1 (66 slide pages) cost
about 7% of a 5-hour limit; figures 0.25% each with generators. Targets below are ESTIMATES until logged in
`SUBJECTS.md`.

## Subject types (read before Stage 0 for any non-theory subject)
This pipeline was written for theory-and-numerical subjects. For programming, systems, mathematics, language and
physics-with-lab subjects read [SUBJECT-TYPES.md](SUBJECT-TYPES.md) and the matching playbook in
[types/](types/): [programming](types/programming.md), [systems-hardware](types/systems-hardware.md),
[mathematics](types/mathematics.md), [language](types/language.md), [physics-lab](types/physics-lab.md).
Source material for the Winter 2026-27 courses: [VTOP-INVENTORY.md](VTOP-INVENTORY.md). Subject list:
[SUBJECTS.md](SUBJECTS.md).

## Tiers
| | Lite (every subject) | Full (only after about 30 sales or strong demand) |
|---|---|---|
| Notes, summaries, question bank, flashcards | yes | yes |
| Figures | slide crops (`tools/fig.js`) | redrawn SVGs (T5 playbook) |
| Interactive models (`viz-paid/`) | none | 3-5 where they aid understanding |
| Past papers | free list only if files exist | worked answers (`papers-pro`) |
| Battles | on (script-built from flashcards) | on |
| Target cost | about 0.5-0.6M tokens, 50-60% of one limit | plus 30-50% of a limit |

## Stage 0 Shell copy (script, about 2%)
Run `new-subject.js <CODE> "<Name>" <prefix>` (SCRIPTS-TO-BUILD.md). It applies the lessons so nobody repeats
them: copy from baeee101-notes (prefixed `_store.js`, lesson 25) with `mv -n` and dotfiles (1, 2); set git
identity (3); create `viz-paid/.gitkeep` (4); delete generated `api/_access.json`, `_battle.json`,
`_content.json` (6); rename code, name, product tag, `STORE_CODE`, `REVIEW_EMAIL`, `storeName`; grep for the old
subject's TOPIC words (5); add the `Projects/.claude/launch.json` entry (7); set `PRICE_PAISE` fallback to the
store list price (20); `"open": false` (S01).

## Stage 1 Extract (script, 0 tokens)
`python tools/build.py` style pypdf dump to `src/text/<module>/<lecture>.txt` with `PYTHONIOENCODING=utf-8` (13);
page renders to `src/pages/`; contact sheets; then `tools/clean-text.py` (T2) to `src/clean/`.
Model step (small): skim contact sheets once per module for image-only tables and real diagrams (9, 11).

## Stage 2 Structure (mid-tier model, about 3%)
Prompt `PROMPTS/02-structure.md`: outline JSON per module (topics, tabs, lecture order, figure pages). Script
`outline-check.js` confirms every lecture heading maps to a topic. Write `content/modules.json` with `lectures` (32).

## Stage 3 Draft (strongest model, about 30-35%)
One lecture per fresh session, prompt `PROMPTS/03-draft-one-lecture.md` (T1). After each: `node tools/validate.js
content/chunk-X.js`, `coverage.js`, commit. Source contradictions stay with a `.note heads-up` (10). No
apostrophes inside single-quoted strings (PLAYBOOK gotcha).

## Stage 4 Verify (scripts + cheaper models, about 5%)
`node tools/validate.js --all` (0 errors), `coverage.js --all`, derive cards and summaries (T3), questions (T4:
`mcq-rotate.js`, `mcq-dump.js`, `mcq-check.py`), `node tools/fig.js` crops with a contact-sheet look (16, 29).

## Stage 5 Polish (strongest model, about 5%)
Only items flagged by Stage 4 and thin-tab warnings. Then `node tools/build.js`, `node tools/qa.js` and
`qa.js --free` once per module (33).

## Quality gates (all must pass before launch)
validate 0 errors · coverage 100% of numbers and named terms · MCQ key agreed by two checkers or reviewed by hand ·
answer letters spread 20-30% each · every figure looked at once on a phone-width sheet · `qa.js` no runtime errors ·
anonymous request for a Pro file returns 401.

## Shortcut risks
Slide crops look worse and may raise copyright questions (redraw best-sellers first) · cheaper-model distractors
can be ambiguous (sample 10 per module) · the outline can miss topics that have no heading (coverage catches
facts, not structure: skim one lecture per module by hand) · fewer audits mean more slips (keep "Report a
mistake" visible; it posts to the store's `/api/community`).

## Launch
`SUBJECT-CHECKLIST.md`. Never list a subject on the store before its site and Pro exist (23).

## Done when
One new subject has gone through every stage with costs logged in SUBJECTS.md.
