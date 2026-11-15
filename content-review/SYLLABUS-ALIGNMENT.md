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
| Applied Chemistry | bachy105-notes | BACHY105 ETH (Fall 2026-27) | Syllabus read (below). Site modules match by name, but the site's m1..m5 chunk order differs; regroup topics under the rows. Module 6 skipped. |
| Basic Engineering | baeee101-notes | BAEEE101 ETH (Fall 2026-27) | Syllabus not read yet. |
| Technical English | baeng101-notes | BAENG101 ETH (Fall 2026-27) | Syllabus not read yet (SPEC says modules follow src/syllabus.pdf: compare with VTOP). |
| EVS | evs-notes | BACHY101: not in the owner's VTOP course list | Source is the MOOVITOL course; already grouped by its 31 lectures inside Weeks 1–5 (Week 6 out of exam). Check whether a VTOP/official syllabus page exists; if not, the lecture grouping stays. |

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

## Order of work (next sessions)
1. BACHY105: finish downloads check (BACHY105-UPGRADE.md), extract text, syllabus rows + past-paper map, fix gaps,
   regroup the sidebar by VTOP rows.
2. Read BAEEE101 and BAENG101 syllabus tabs (owner signed in), then the same steps per site.
3. EVS: confirm the syllabus source; keep the lecture grouping if no VTOP page exists.
Free models (FreeLLMAPI) do the question → tab mapping and row → topic suggestions; scripts verify; Claude decides.
