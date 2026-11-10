Status: PLAN (nothing built). Parent: `../SUBJECT-TYPES.md`.

# Playbook: physics with a lab (BAPHY105 Engineering Physics)

About 181-190 uploads on VTOP, first row from Fall 2026-27 (rows, syllabus and lab option not read). This is the
closest type to the existing Chemistry and Basic Engineering sites, so it should be the first new subject.

## Content format changes
- Tabs per topic: The idea, Derivation, Formula and units, Worked numerical, Applications. Derivation tabs use
  `ol.proof`-style stepped reveal (shared with the mathematics playbook) and `.eq` blocks; adopt build-time
  KaTeX (B5) only if derivations need integrals or matrices that `.eq` cannot show.
- `formulas[]` gains optional machine-readable fields: `{ name, html, note, vars:[{ sym:'λ', unit:'m',
  label:'wavelength' }], solve:'lambda = h / (m * v)' }`. Old entries without `vars` still work.
- Lab topics (B4): `{ kind:'lab', exp, aim, apparatus, formula, procedure, table:{ cols, sampleRows },
  calc, result, errors, viva:[{q,a}] }`.
- Questions: existing `numerical` and `interpret`; `type-in` (B2) with a tolerance (`accept: { value, tol:0.02,
  unit:'nm' }`).

## App code to extend
- `app/js/app.js` `viewFormulas()`: when a formula has `vars`, show a calculator: inputs with unit pickers
  (nm, µm, m; eV, J), SI conversion, result with significant figures via the existing `sci()`/`fmt()` helpers in
  `viz-core.js`. One generic calculator replaces hand-built ones per subject (lesson 5).
- `viewLab()`: generic lab page from B4 data, with an observation table the student can fill in and a
  least-count and percentage-error helper.
- `viz-paid/`: 3-5 simulations (Full tier only) on `viz-core.js`: interference and diffraction pattern,
  photoelectric effect, particle in a box, laser levels or fibre acceptance angle, depending on the syllabus.
  `wavelengthToColor` already exists in a shell widget file and can be reused.
- `tools/validate.js`: the new fields; new `tools/check-formulas.js`: every worked numerical recomputed from
  `solve` and compared with the stated answer.

## Unchanged
Recall, flashcards, quiz, mock tests, papers, battle, summaries, figure generators, paywall.

## Cost
Lite 50-60% (the largest source set; pick one faculty's files to keep extraction small). Unit calculator 4%
once (reusable by Chemistry and Basic Engineering). Lab pages (B4) 5% once plus about 0.5% per experiment.
Full tier later: simulations at about 3% each by hand, less on shared helpers.

## Quality risks
Unit slips and constants that differ from the faculty's values (take constants from the slides, list them once);
derivations with a missing step; lab readings invented to look real (label sample rows as examples and state
the source); modern-physics simulations that mislead if simplified too far (state the assumptions).

## Free and Pro
Free: notes, one derivation per module, one sample experiment, formula list without calculators.
Pro: calculators, all derivations stepped, numericals with working, lab calculations and viva answers,
simulations.

## Build order
1. Finish the VTOP inventory for this course and choose one faculty set. 2. Lite subject through PIPELINE.md.
3. Unit calculator in `viewFormulas()`. 4. B4 lab pages. 5. Simulations after about 30 sales (Full tier rule).
