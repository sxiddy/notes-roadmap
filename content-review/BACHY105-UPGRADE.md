# BACHY105 (Applied Chemistry) upgrade plan

Started 16-11-2026. Standard: new-subjects/PIPELINE.md, "Content standard". Module 6 (Contemporary Topics) is
skipped: not examined.

## Status
- VTOP read (owner signed in): 310 uploads (Fall 2026-27: 119, Fall 2025-26: 130, Winter 2025-26: 61), 17 faculty
  in the current semester. 63 of them are question banks, model papers, CAT QPs, sample questions or numericals.
- Syllabus (VTOP "Course Syllabus Modules"): M1 thermodynamics and kinetics (9 sessions), M2 energy devices (8),
  M3 functional materials (10), M4 spectroscopy and diffraction (6), M5 computational chemistry (10), M6
  contemporary (skip). Matches the site's five modules.
- Deck set chosen: one faculty's complete Fall 2026-27 set (Modules 1–4 of 09-10-2026, Module 5 PPT of
  22-10-2026).
- Downloads: blocked after the first file until the owner allows multiple downloads from vtop.vit.ac.in in Chrome.
- The site's original source decks (src/) were deleted after building, so the audit uses the fresh download.

## Steps
1. Download the deck set and the 63 exam files to `bachy105-notes/src/vtop/` (gitignored), extract text.
2. Past papers: CAT/FAT from papers.codechefvit.com and examcooker.acmvit.in, plus VTOP CAT QPs.
3. Map every syllabus topic and every past-paper question to a site tab (`content-review/bachy105-map.md`).
4. Fix only gaps and keyword-only tabs (one agent at a time, never Haiku). Add models only for hard topics that
   lack one (the site already has 19).
5. validate, build, qa; commit; push only when the owner says "push". Delete the VTOP downloads when done.
