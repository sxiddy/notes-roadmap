# Every notes site: the owner's conditions (16-11-2026) and what is left per site

Run one site at a time (one agent at a time, never Haiku). Check the 5-hour limit before each step and stop at 95%.
Commit when a site passes; push only when the owner says "push". Never write faculty/student names, register or
class numbers into any file.

## The conditions (all must hold on every site)
1. **Syllabus complete.** Every item of the official syllabus (VTOP "Course Syllabus Modules" or `src/syllabus.pdf`)
   has notes. Skip modules not examined: Contemporary Topics everywhere; BAEEE101 Module 5.
2. **Organised like the syllabus.** `syllabusRows` in `content/modules.json`; sidebar and module pages grouped by row;
   `node tools/syllabus-check.js` passes. (Port: see SYLLABUS-ALIGNMENT.md.)
3. **Every past-paper question answerable.** Papers from papers.codechefvit.com (`/api/papers?subject=<Name [CODE]>`
   gives file URLs) and examcooker.acmvit.in/past_papers/<CODE>, plus VTOP CAT QPs and question banks. Read scans
   as images. Write `src/PAPER-MAP.md` (question → topic/tab → covered/GAP); fix every GAP.
4. **Exam-complete, not keyword-short.** Full-sentence definitions, every list item explained, table cells as
   clauses, `.note exam` outlines. Rewrite only what fails; leave good tabs alone. (EVS: exam is MCQ-only, so the
   standard there is precise facts + a correct, complete MCQ bank.)
5. **Real diagrams.** Never describe a diagram in words instead of showing it; slide crop or exam-correct SVG.
6. **Paper answers link to the notes.** Every answer in the site's past-papers section carries "Notes: Module ›
   Topic › Tab" linking to the tab that answers it.
7. **Interactive models only for hard topics** (processes, mechanisms, look-alike concepts, input-driven numbers).
8. **Numbers verified by script.** Every worked numerical recomputed before it is written.
9. **Checks before commit:** `node tools/validate.js --all` (no new errors), `node tools/build.js`,
   `node tools/qa.js` (no runtime errors; `npm install` in tools/ if jsdom is missing), phone-width look at new pages.
10. **Free models (FreeLLMAPI) for light, script-checkable jobs** (question → tab mapping, MCQ key checks).

## Per site: what is left
### BAEEE101 Basic Engineering (`baeee101-notes`) — agent running 16-11-2026
Papers (40 in src/papers-cc), gaps, real diagrams (fix existing word-only diagrams), references, rows. Not pushed.

### BACHY105 Applied Chemistry (`bachy105-notes`) — DONE 16-11-2026, d6576e3 pushed
- All 22 GAPS items present (5–6 added: Carnot theorem tab, adsorption ΔS < 0, absolute entropy; 7 verified).
- Condition 3: 14 papers (incl. new FAT B1+TB1 Nov 2026 with model answers), 91 questions, all covered:
  `content-review/bachy105-paper-map.md` (copy of `src/PAPER-MAP.md`, which is gitignored). ExamCooker's 8 papers all present.
- Condition 5: 7 new SVGs replacing word-only diagrams (AFC cell, O₂ MO, Maxwell–Boltzmann, Hc–T, Type I/II, Morse, HCN→HNC).
- Condition 6: every answer has "Notes: Module › Topic › Tab" links; `validate.js` now fails an actual-paper question without `notes[]`.
- MCQs: +8 (M1), keys rebalanced 11/11/11/12; +7 flashcards. Checks: validate 124 old errors only, build, qa no runtime errors, syllabus-check OK.
- Left (minor): two warned thin tabs in chunk-K (`m3-conducting-materials` "Comparing the metals", `m3-insulators` "Uses & examples").
### BAENG101 Technical English (`baeng101-notes`) — NEXT (not started 16-11-2026; stopped at 87% of the 5-hour limit)
- Done: rows (2); word bank of all 100 definitions (m3-word-bank, pushed).
- Left: condition 3 (CodeChef + ExamCooker BAENG101 papers; also `src/papers`, `src/papers-dl`, and
  ~/Downloads BAENG101-CAT2-*.txt and "BAENG101 - CAT I - Question pattern.docx"), condition 6, condition 4
  audit of thin tabs, condition 5 (any described-only figures).
### BACHY101 EVS (`evs-notes`) — MCQ-only exam
- Done: audit + 8 models (pushed); lecture grouping (its syllabus = the course's 31 lectures).
- Left: MCQ bank covers every fact added in the audit (one or more questions per new fact, keys checked by a
  script or free model, answer letters spread 20–30 %); any past EVS MCQ papers (CodeChef/ExamCooker "BACHY101" or
  the EVS course name) mapped; condition 6 for any papers section.
