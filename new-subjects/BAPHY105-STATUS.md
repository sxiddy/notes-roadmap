# BAPHY105 Engineering Physics: build status

Repo: `Projects/baphy105-notes` (local only, not pushed, no Vercel project yet). Store: NOT listed (lesson 23).
Preview: `Projects/.claude/launch.json` entry `baphy105-notes-app` (port 4185).

## Done (16-11-2026)
- Stage 0: shell copied from baeee101-notes HEAD (`git archive`, so the other session's uncommitted work was not
  copied), rebranded BAEEE101 → BAPHY105, `_store.js` prefix `baphy105:`, `PRICE_PAISE` fallback 2000 (₹20),
  `PRICE_RS || 20`, `api/_mode.json` open:false, old content/figures/viz/generated api JSON removed,
  `viz-paid/.gitkeep`, git identity set. Commit fcf33f1.
- VTOP syllabus read into `content/vtop-syllabus.json`. The course is **quantum mechanics**, not optics/lasers:
  M1 Wave-particle duality (dual nature, YDSE, blackbody, Planck, de Broglie, wavefunction, Stern–Gerlach);
  M2 Mathematical foundations (linear vector space, basis, orthonormal sets, Hilbert space, Dirac notation, inner/outer
  products, tensor product, linear operators, Hermitian/unitary/projection, eigenvalues, Pauli matrices, commutators);
  M3 Postulates (observables, expectation values, measurement, sequential Stern–Gerlach, TDSE, TISE, numericals);
  M4 Applications (1D well, 3D well, eigenvalues/eigenfunctions, degeneracy, tunnelling, tunnelling probability,
  numericals); M5 Quantum computing (qubits, single-qubit states, Bloch sphere, two-qubit systems, entanglement,
  Bell states, single-qubit gates, Pauli and Hadamard, CNOT, generating entangled states); M6 Contemporary: skipped.
  The physics-lab playbook (optics, lasers, fibres) does not fit; lab (ELA) not read.
- VTOP Course Page: 184 uploads read (2 server pages of 100). Faculty coded F1–F23 (names not recorded).
  Chosen set: **F2, Fall 2026-27** (complete: lecture notes for Modules 1–5 + decks on Planck, de Broglie, Compton,
  wavefunction, linear algebra, Pauli matrices, postulates, expectation values, sequential S–G, particle in a box,
  tunnelling/STM, Module 5 deck, SWAP gate). Plus every question/key file: Fall 2025-26 CAT-2 C1 key, solved
  problems; Winter 2025-26 CAT-1 keys (C1, C2), CAT-2 keys (F1, F2, one more), CAT-1 portion, FAT blueprint,
  Quiz-2 QB, M1 numericals, particle 1D/3D problems, book (105 p), blackbody book notes; Fall 2026-27 assessment
  tools, quiz-1 info. 34 files in `baphy105-notes/src/vtop/` (gitignored). Delete when the site is finished.
- Past papers in `src/papers/`: CodeChef `Engineering Physics [BAPHY105]` 16 files (CAT-1/2, FAT; Fall 2025-26,
  Winter 2025-26, Fall 2026-27 F1) + ExamCooker 3 unique (9 duplicates removed). Many are scans: page images in
  `src/pages/<file>/pNN.png`, text in `src/text/`.

## Next (in order)
1. Read the five lecture-notes PDFs (`src/text/vtop__FALL2026-27_Lecture-notes-*.txt`) + deck images → write
   `content/modules.json` with `syllabusRows` (port the grouping from bachy105-notes: SYLLABUS-ALIGNMENT.md) and one
   chunk per module (`content/chunk-1a.js` …). Exam-complete text; derivations stepped; real diagrams as SVG
   (`figs-svg/`); every numerical recomputed by a script (`tools/check-numericals.js`, to write).
2. Paper answers: bachy105's uncommitted `notes: ['topic.tab']` field + `notesLinks()` in app.js and the validate
   rule (lines ~250) are the pattern for condition 6. Write `src/PAPER-MAP.md`, `content/papers-free.js`/`papers-pro.js`.
3. Questions + flashcards per module, answer letters spread 20–30 %.
4. App leftovers from the BAEEE101 shell still to replace: syllabus map `full:` in app.js (~line 2241), formula
   sheet and lab-bench wording, `SPEC.md`, `README.md`, home tiles.
5. validate / build / qa / syllabus-check / phone look → commit. Then store entry (unpushed).

## Cost
Stage 0 + sourcing: about 25% of a 5-hour window (window shared with two other sessions, so the share is approximate).
