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

## Content standard (owner, 16-11-2026: applies to every notes site, new and existing)
1. **Sources from VTOP.** The owner signs in to VTOP in their Chrome (Claude never types the password or captcha).
   Course Page lists 300+ uploads across semesters: read every page, not just the first 100. Download one complete,
   current faculty deck set for all modules, plus every question bank, model paper, CAT/FAT paper, sample-question
   and numericals file. Chrome must be allowed to download multiple files from vtop.vit.ac.in (owner clicks once).
   Delete the downloads once the site is finished.
2. **Syllabus = VTOP "Course Syllabus Modules" tab.** Every topic listed there must have notes. A "Contemporary
   Topics" module is not examined: skip it.
3. **Past papers must be answerable.** Collect CAT and FAT papers from papers.codechefvit.com
   (`/catalogue?subject=<Subject+[CODE]>`) and examcooker.acmvit.in (`/past_papers/<CODE>`), plus the VTOP CAT QPs.
   Make a question → topic/tab map; every question must be answerable from the notes. Any gap is a content fix.
4. **Exam-complete, not keyword-short.** A student must be able to write a full answer from the notes: full-sentence
   definitions, each list item explained (what, how/why, the source's example), table cells as clauses, an exam
   outline per topic. Rewrite only what fails this (missing facts or bare keyword lists); leave good tabs alone.
   Tools from evs-notes: `tools/coverage.js` (source sentences missing from notes) and `tools/terse.js`
   (keyword-only tabs).
6. **Free models for light work** (owner, 16-11-2026). Use FreeLLMAPI (`Projects/external-repos/freellmapi`,
   start with preview_start "freellmapi") wherever the job is small and checkable: mapping past-paper questions to
   tabs, MCQ answer checks, flashcards from `<b>` terms, text clean-up, finding source quotes. Every output goes
   through a script check (quotes verbatim in the source, JSON parses, answers present in the notes). They never
   write what students read. In Nov 2026 most single-model routes returned 429; the router's `fusion` model
   worked at 1 request every ~10 s (evs-notes `tools/freellm.py`, `tools/mcq-check.py`).
7. **Real diagrams, never word descriptions** (owner, 16-11-2026). Students copy diagrams into exam answers: every
   diagram/circuit/plot/projection must be shown (slide crop or exam-correct SVG); prose may explain, never replace it.
8. **Past-paper answers link to the notes** (owner, 16-11-2026). Every answer in the papers section carries a
   reference "Notes: Module › Topic › Tab" (link) to the tab that answers it.
9. **Skip modules not examined**: Contemporary Topics everywhere; BAEEE101 Module 5 (Bio-Inspired Design) too.
5. **Interactive models only for hard topics**: processes, mechanisms, look-alike concepts, numbers that change
   with an input. Never decorative. Every label and number from the source. Check each with `tools/viz-snap.js`.

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
validate 0 errors · coverage 100% of numbers and named terms · every VTOP syllabus topic has notes · every past
CAT/FAT question maps to a tab that answers it · `terse.js` flags only complete data tables · MCQ key agreed by two checkers or reviewed by hand ·
answer letters spread 20-30% each · every figure looked at once on a phone-width sheet · `qa.js` no runtime errors ·
anonymous request for a Pro file returns 401.

## Shortcut risks
Slide crops look worse and may raise copyright questions (redraw best-sellers first) · cheaper-model distractors
can be ambiguous (sample 10 per module) · the outline can miss topics that have no heading (coverage catches
facts, not structure: skim one lecture per module by hand) · fewer audits mean more slips (keep "Report a
mistake" visible; it posts to the store's `/api/community`).

## Launch
`SUBJECT-CHECKLIST.md`. Never list a subject on the store before its site and Pro exist (23).

Launch steps learned on BAPHY105 (18-11-2026). Do every one before listing the subject on the store:
1. **Google sign-in.** Google Cloud (the account that owns project "BACHY105 Notes"), Google Auth Platform → Clients →
   OAuth client 1031135588562-kg77…, then add the origin `https://<code>.vercel.app` to Authorised JavaScript origins and
   `https://<code>.vercel.app/api/login` to Authorised redirect URIs, and Save. Without this, sign-in fails with
   "Error 400: redirect_uri_mismatch".
2. **Vercel env.** Copy STORE_SECRET and ADMIN_KEY from the store project (getnotespro): its values are readable.
   Never `vercel env pull` from a subject site: its values are "sensitive" and come back as the text "[SENSITIVE]".
   Connect the shared Upstash store (store_wToywU9aGFJgKpLi) for the KV_* and REDIS_URL vars. Give the site its own
   SESSION_SECRET and REVIEW_CODE.
3. **Price.** The store shows a sale price cut from the list price (PRODUCTS.price in vit-notes-store api/_lib.js).
   To sell at ₹20 like the others, set the list price to 3000.
4. **Check live.** /api/session returns 200 with the right price. /api/stats returns 200 with the store's keys and
   401 without. A real Google sign-in reaches the site.

## Done when
One new subject has gone through every stage with costs logged in SUBJECTS.md.
