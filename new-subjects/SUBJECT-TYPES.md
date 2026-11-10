STATUS 11-11-2026: plan written, nothing built. Done: this file, five playbooks in `types/`, SUBJECTS.md rows,
README and PIPELINE links. Not done: VTOP inventory is partial (3 of 13 class options listed, see
`VTOP-INVENTORY.md`); shell code was surveyed in baeee101-notes by function name and SPEC.md, not line by line, so
every "extend function X" below must be confirmed by reading that function first. Exact next step: owner answers
the three decisions at the end, then finish the inventory, then build prototype P1.

# Subject types: how non-theory subjects fit the notes shell

## What the shell can and cannot do today (baeee101-notes, the reference shell)
- Content: `content/chunk-*.js` topics with `tabs[{title, html}]`, `flashcards[{front, back}]`, `formulas[]`.
  Tab HTML is an allow-list checked by `tools/validate.js`: `p h3 h4 b em sub sup br ul ol li table thead tbody tr
  th td div span figure details summary small code`. There is no `pre`, no syntax highlighting, no copy button.
- Maths: `.eq` blocks with `sub`/`sup` and `.frac` spans. No KaTeX or MathJax anywhere.
- Questions: `content/q-*.js`, types `decide predict explain troubleshoot design numerical interpret`, four
  options or open. `tools/build.js` splits answers into `qa-*.js` (Pro).
- Interactive models: `viz-paid/viz-<m>.js`, each `VIZ.register('id', {...})` on `app/js/viz-core.js` (helpers
  `svg el css fmt mount set sci`). One free file in `app/js/viz/`. No shared step player or timeline.
- Views in `app/js/app.js`: `viewTopic viewSummary viewBank viewQuiz viewCards viewFormulas viewLab viewMock
  viewPapers viewSyllabus viewBattle viewRevise viewMine`. `viewLab` and `viewFormulas` are subject-specific.
- No audio, no speech, no code execution, no text-answer checking (quiz is option index only).

## Taxonomy
| Type | Courses | Playbook | Core new piece |
|---|---|---|---|
| Theory and numerical (existing) | Chemistry, Basic Engineering, English, EVS | PIPELINE.md | none |
| Programming and algorithms | BACSE104, BACSE105 (BACSE101 Python later) | `types/programming.md` | code blocks, code questions, step player |
| Systems and hardware | BACSE103, BACSE106 | `types/systems-hardware.md` | simulator core on the step player |
| Mathematics | BAMAT205 (BAMAT101 later) | `types/mathematics.md` | build-time KaTeX, checkers |
| Language | BAESP101 | `types/language.md` | typed-answer drills, spaced repetition, speech |
| Physics with a lab | BAPHY105 | `types/physics-lab.md` | unit-aware calculators, lab pages |

## Shared building blocks (build once, in this order)
| Id | Block | Files | Used by | Cost (% of a 5-hour limit, estimate) |
|---|---|---|---|---|
| B1 | Code block: `<pre class="code" data-lang>` + copy button + small tokenizer | `validate.js` allow-list, `hydrate()` in `app.js`, CSS | programming, systems, lab pages | 4% |
| B2 | Typed and code question types (`output`, `fill`, `bug`, `type-in`) | `validate.js`, `build.js` answer split, `qCard()`/`answerHTML()`/`viewQuiz()` | programming, language, maths | 6% |
| B3 | Step player `app/js/viz-step.js` (frames, prev/next/play, speed) | new file beside `viz-core.js` | programming, systems, maths | 4% |
| B4 | Lab pages (`part: "ELA"` modules, `kind: 'lab'` topics) | `modules.json`, `viewLab()`, `buildSidebar()`, `build.js` | every ETH+ELA course | 5% |
| B5 | Build-time KaTeX | `build.js`, self-hosted CSS and fonts | maths, physics, systems | 4% |
| B6 | Spaced repetition (already planned as N3) | `features-notes/` N3 | language first, then all | 8% (N3) |

Total one-off shell work: about 31%, strongest model, verified by `validate.js`, `qa.js` and a phone check.
Port each block into the reference shell (baeee101-notes) on a branch so `new-subject.js` copies it everywhere.

## Theory and lab (ETH and ELA) on the site and in the store
- One site and one product per course code. Students register for ETH and ELA together and think of it as one
  course; two products would double support and halve perceived value.
- Site: the sidebar gets two groups, "Theory" (modules 1-5) and "Lab" (experiments). Lab pages are B4: aim,
  theory in three lines, procedure or code, expected output or observation table, result, viva questions.
- Free and Pro: lab aim, procedure and one full sample experiment free; solutions, outputs, viva answers Pro.
- Assessment differs: theory has CAT1, CAT2, FAT; lab has continuous assessment and a lab FAT. The exam
  countdown (N1) needs both dates; mock tests stay theory-only; lab gets a "viva drill" built from viva questions.
- Store listing: title "<Course> (Theory + Lab)", two count lines ("5 modules of notes", "12 lab experiments
  with solutions and viva"), and a badge "Lab included". Theory-only courses (BAMAT205) show no badge. Do not
  list lab content until the lab pages exist (lesson 23).
- Integrity rule: lab solutions are for understanding and viva preparation. Add one line on every lab page
  telling students to write their own record. The owner should decide how far to go (decision 3).

## Cost summary per subject (Lite baseline is 50-60%; model split as in MODELS-AND-BUILD-METHODS.md section 4)
| Type | Extra over Lite | Notes |
|---|---|---|
| Programming | +15-20% first subject, +8-10% next | code verified by compiling offline, not by a model |
| Systems and hardware | +12-18% | 4-6 simulators at 1-1.5% each once B3 exists |
| Mathematics | +10-15% | checkers are small; proofs cost drafting tokens |
| Language | +10% and far fewer note pages | source material is the risk, not tokens |
| Physics with lab | +5-8% | nearest to today's shell |

## Prototypes to validate before committing (propose only, none built)
- P1 (first): one BACSE104 topic (pointers) with B1 code blocks, one trace table and five `output` questions,
  built in a copy of the shell. About 6%. Pass: validate 0 errors, copy works on a phone, answers stay out of the
  free bundle (anonymous request returns 401).
- P2: step player with one bubble-sort widget and one FCFS Gantt widget sharing it. About 5%.
- P3: one BAMAT205 tab rendered with build-time KaTeX; measure added bytes. About 2%.
- P4: ten Spanish verbs with typed conjugation drill and browser speech; test on two phones. About 3%.

## Decisions the owner must make
1. Runnable code: recommended NO in-browser execution for C and C++ (see `types/programming.md`); ship verified
   traces and outputs instead. Confirm, or ask for the Python-only Pyodide option.
2. Source material: approve downloads from VTOP, one faculty set per course; name a source for Spanish.
3. Lab solutions: full solutions behind Pro, or hints plus viva only (academic-integrity risk to the brand).
