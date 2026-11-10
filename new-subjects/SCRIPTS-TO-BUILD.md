Status: TODO

# Helper scripts to build once and reuse for every subject

Where: a new folder `Projects/notes-tools/` (own private repo), so the four sites and all new ones use one copy.
Each script takes the subject folder as its first argument, prints a short report, exits non-zero on failure,
and never needs a model. Write regex-heavy code with the Write tool, not shell heredocs (PLAYBOOK gotcha). Use
function replacers in `String.replace` (lesson 27).

| Script | Reuse from | Spec |
|---|---|---|
| `new-subject.js <CODE> "<Name>" <prefix>` | `baeee101-notes/tools/rebrand.js` (read it first) | Stage 0 of PIPELINE.md, every bullet. Refuses to run if the target folder exists. Ends by printing leftover old-subject words found by grep. |
| `extract.py <subject>` | `evs-notes/tools/build.py` (pypdf dump) | PDFs/PPTX in `src/pdfs/` to `src/text/<module>/<lecture>.txt` with `=== pN ===` page markers; page PNGs to `src/pages/`; one contact sheet per 20 pages in `tmp/sheets/`. |
| `clean-text.py <subject>` | new | T2: delete lines repeating on over 40% of pages, page numbers, build-up duplicate slides. Output `src/clean/`. Prints words before/after. Deletes only; never rewrites. |
| `outline-check.js <subject>` | new | Reads `tmp/outline.json` and `src/text`; fails if a slide title (first line after a page marker) matches no topic or tab title (fuzzy: lowercase, letters and digits only). |
| `coverage.js <subject> [chunk]` | `baeee101-notes/tools/check-m1.js` .. `check-m4.js` (per-module copies today: merge into one) | Every number (with unit), year, formula token and Capitalised multi-word term in the ORIGINAL `src/text` of a lecture must appear in that lecture's chunk (tabs, notes, figure captions). Prints missing items with page numbers. An allow-list file `src/coverage-skip.txt` for lecturer names and course codes. |
| `derived-check.js <subject> <module>` | new | T3 checks on flashcards and summaries. |
| `mcq-rotate.js <q-file>` | new | Rewrites option order so correct answers are evenly spread across A-D; keeps ids; refuses if the file is not in the row-array format. |
| `mcq-dump.js`, `mcq-check.py`, `freellm.py` | `evs-notes/tools/` as is | Dump questions without keys; two free models answer from notes; print disagreements. `freellm.py` already waits for the router's retry time. Key is read from an env var or a gitignored file; never printed. |
| `figs/` (`lib.js`, `table.js`, `phone-preview.js`, `todo.js`, `PLAYBOOK.md`) | `evs-notes/tools/figs/` | Make `todo.js` read the figure-entry pattern from a small config (lesson 35). Add `svg-lint.js`: XML-parse every file, fail on duplicate attributes (lesson 34) and fixed colours. |
| `launch-check.js <site url> <CODE>` | new | Anonymous: `/api/session` shows sign-in on, not free, right `storeCode`, price equal to the store's; a Pro file gives 401; the store `/api/session` lists the code. Prints PASS/FAIL lines. No secrets needed. |
| `sync-shell.js <subject>` | new | Diff `api/*.js`, `app/js/*.js`, `tools/*.js` against the reference site with `--strip-trailing-cr`, ignoring the known per-subject constants; prints drift. Run before every shared fix. |

## Build order
`new-subject.js`, `extract.py`, `clean-text.py`, `coverage.js` first (needed by Stage 0-3); the rest before Stage 4.

## Cost
About 15-20% of one 5-hour limit in total (estimate), paid once.

## Tests
Each script gets a tiny fixture under `notes-tools/fixtures/` (one lecture of EVS text, one chunk, one q-file)
and a `node --test` or `python -m unittest` file. Run them against evs-notes as the reference: `coverage.js`
must report 0 missing for EVS Module 1.

## Done when
All scripts exist with tests, EVS passes them, and PIPELINE.md commands use their real names.
