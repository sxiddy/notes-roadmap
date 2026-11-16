# Align every notes site with the VTOP syllabus (owner, 16-11-2026)

Rule: every topic in VTOP's "Course Syllabus Modules" tab has notes, and the site is organised the same way:
module → VTOP syllabus rows (in VTOP's order and wording) → our topics. "Contemporary Topics" modules are skipped
(not examined). Applies to all sites, current and future.

## How (per site, about 2–4% of a 5-hour window each, plus content fixes)
1. Owner signs in to VTOP in Chrome. Course Page → pick the course (ETH) → open "Course Syllabus Modules".
   Read the table with JS (rows: module title, then numbered topic rows) and save it to
   `<site>/content/vtop-syllabus.json` as `[{module, title, rows:[{n, text}]}]`. Wait ~4 s after changing the
   course: the table reloads slowly (an early read returns the previous course's table).
2. In `content/modules.json`, give each module a `syllabusRows` list: `{n, text, topics:[topic ids]}`. This is the
   same shape as EVS's `lectures` (`{n, title, topics}`), so the sidebar grouping in evs-notes `app.js`
   (`.sb-lec` / `.lec-head`, lectures passed through `tools/build.js`) can be reused: group by VTOP row.
3. A script (`tools/syllabus-check.js`, to write) fails if any VTOP row has no topic, or any topic sits under no row.
   Missing row = write the notes (Content standard, PIPELINE.md). Module titles and order follow VTOP.
4. Rebuild, validate, qa, phone check of the sidebar. Commit; push only on "push".

## Per site
| Site | Repo | VTOP course | Status |
|---|---|---|---|
| Applied Chemistry | bachy105-notes | BACHY105 ETH (Fall 2026-27) | DONE and pushed 16-11-2026: `syllabusRows` in modules.json (34 rows, some VTOP rows merged where one topic teaches both, e.g. "4–6"), sidebar and module pages grouped by row (`.sb-row`, `.row-head`), `tools/syllabus-check.js` passes. |
| Basic Engineering | baeee101-notes | BAEEE101 ETH (Fall 2026-27) | Syllabus read from `src/syllabus.pdf` (official). GAPS, no notes yet: **all of Module 5 Bio-Inspired Design** (biomimicry principles; Velcro/burrs, self-healing materials/skin, termite-mound ventilation, insect drones and soft robotics, neural networks/brain; bio-inspired problem solving); M4 refrigeration and air conditioning (deck on VTOP: Lecture18, 17-10-2026), four-bar mechanism and inversions, hexapod mechanisms, mechanical components and motion control, robotics and automation, pneumatic and hydraulic actuators, fuel cells and sustainable energy (only partly); M3 residential floor plan (IS SP 7), isometric/perspective (check depth), freehand sketching (check depth). IN PROGRESS 16-11-2026 (agent): 40 CodeChef papers in src/papers-cc + ExamCooker, paper map src/PAPER-MAP.md, missing topics written (M5 from syllabus + papers, flagged: no slides), grouping ported. Not pushed yet. |
| Technical English | baeng101-notes | BAENG101 ETH (Fall 2026-27) | Syllabus read from `src/syllabus.pdf`: every item already has notes. DONE and pushed 16-11-2026: grouped by the 5 modules' syllabus rows (summarising/abstracts kept under an "Also taught" row). Past-paper check still to do. |
| EVS | evs-notes | BACHY101: not in the owner's VTOP course list | Exam is MCQ-only (owner, 16-11-2026): priority is the MCQ bank (every added fact has questions, keys checked), not written outlines. Grouped by its 31 lectures inside Weeks 1–5 (Week 6 out of exam). |

How to port the grouping to another site (about 10 minutes): copy from bachy105-notes the `ROW`/`rowHead` block in
`app/js/app.js` (just before the Free/Pro block) and the two `rowHead(...)` calls (sidebar and module page), the
`syllabusRows` field in `tools/build.js` (modules.js line), the three CSS rules at the end of `app/css/app.css`, and
`tools/syllabus-check.js`. Then add `syllabusRows` to `content/modules.json`.

## BACHY105 syllabus (VTOP, read 16-11-2026)
1. Chemical thermodynamics and kinetics: 1 introduction, terminologies · 2 zeroth law, first law, enthalpy and heat
   capacity · 3 applications of the first law, second law · 4 Carnot cycle · 5 third law, Gibbs–Helmholtz equation ·
   6 chemical kinetics: rate equation, order, integral rate equations, half-life · 7 collision theory, effect of
   temperature and catalysts · 8–9 numericals.
2. Energy devices: 1 electrochemical and electrolytic cells, electrode materials with examples · 2 Nernst equation,
   numericals · 3 classification of batteries · 4 chemistry of Li-ion secondary batteries · 5 fuel cells (H₂–O₂,
   solid oxide) · 6 silicon photovoltaic cells · 7 photoelectrochemical cells · 8 dye-sensitized cells.
3. Functional materials: 1 electrical materials, Ohm's law, resistivity · 2 classification of metals and
   semiconductors · 3 intrinsic and extrinsic Si, insulators, superconductors · 4 para- and diamagnetic · 5
   ferromagnetic · 6 antiferromagnetic, applications in storage devices · 7 liquid cooling: coolant types, thermal
   conductivity, applications · 8 nanomaterials introduction · 9 top-down and bottom-up synthesis · 10 bulk vs nano
   gold, quantum dots.
4. Spectroscopic and diffraction techniques: 1 fundamentals of spectroscopy · 2 fundamentals of diffraction ·
   3 UV-Visible principle and instrumentation · 4 UV-Visible applications · 5–6 powder XRD, crystallite size,
   numericals.
5. Computational chemistry: 1–2 introduction to quantum mechanical calculations · 3 MO theory, atomic and molecular
   orbitals · 4 orbital interactions, bonding · 5 potential energy curve and surface · 6 electrostatic potentials ·
   7 geometry optimisation · 8 frequency · 9 reaction mechanism · 10 AI to screen semiconductors.
6. Contemporary topics: skipped.

## Order of work (next sessions; updated 16-11-2026)
1. BACHY105: review the content-fix agent's work (src/GAPS.md, 22 items), validate/build/qa, commit. Then MCQs and
   flashcards for the new facts; delete src/vtop and src/papers when the site is final (owner: delete VTOP downloads).
2. BAENG101: port the grouping (above) + syllabusRows; past papers (CodeChef/ExamCooker) answerability check.
3. BAEEE101: owner signs in to VTOP (Claude in Chrome must be connected); download M4B/M5 decks + question banks;
   write the missing topics listed above; past papers; port the grouping.
4. EVS: MCQ coverage of the facts added in the audit.
Free models (FreeLLMAPI) do the question → tab mapping and row → topic suggestions; scripts verify; Claude decides.
