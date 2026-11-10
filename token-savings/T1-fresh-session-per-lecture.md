Status: TODO

# T1 One fresh session per lecture, with a 30-line brief
Estimated saving: 25-35% of drafting tokens (estimate, not measured)

## Waste today
Every tool call re-reads the whole session. EVS Module 1 (66 pages) took one session from 20% to 75% context
(lesson 14); a figures session ended near 300k context. Sessions also start by reading `PLAN.md` + `SPEC.md`
(about 480 lines in evs-notes) and the lessons file.

## Change
1. Write `<subject>/BRIEF.md` once per subject, 30 lines maximum: chunk format with one tiny example, the writing
   rules that the validator does not already enforce, id scheme, banned phrases, "facts only from the given text".
   Start from `baeng101-notes/src/drive-text/BRIEF.md`.
2. Each drafting session gets: BRIEF.md, ONE lecture's cleaned text (see T2), the outline entry for that lecture.
   Nothing else. Use `new-subjects/PROMPTS/03-draft-one-lecture.md`.
3. The session ends after: write chunk, `node tools/validate.js content/chunk-X.js`, coverage check, commit,
   one status line in the subject's `STATE.md`.
4. Do not read `PLAN.md`, `SPEC.md` or other chunks in a drafting session. Do not explore the repo.

## Measure
`get_usage` before and after each lecture session; log "pages, % of limit" in `new-subjects/SUBJECTS.md`.
Baseline: about 0.1% of a 5-hour limit per slide page (EVS M1: 66 pages, 7%). Target: 0.07% or less.

## Quality guard
`tools/validate.js` 0 errors and the coverage script (every number and named term of the lecture text appears
in the chunk) must pass before the commit. If the session needed a second file to write correctly, add the
missing rule to BRIEF.md instead of reading the big docs.

## Done when
Tried on one module of a new subject, numbers logged, BRIEF.md template saved in `new-subjects/PROMPTS/`.
