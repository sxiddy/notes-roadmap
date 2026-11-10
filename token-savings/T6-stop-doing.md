Status: TODO

# T6 Things to stop doing
Estimated saving: 10-20% of a subject overall (estimate)

| Stop | Why | Do instead |
|---|---|---|
| Whole-module audit passes by a model after writing | Re-reads everything; sub-agent audits also raised false "unverifiable" flags when text extraction was lossy (lesson 31) | Run the coverage script and `validate.js`; a model looks only at what they flag, with the page image open |
| Free-model fact lists on whole lectures | Failed with 413 (too large) and 429 (rate limit); nothing was saved | Drop it. The coverage script does this job for free |
| Cache warmers | They re-read the session on every ping and count toward the limit (PLAN.md note) | Short sessions (T1) |
| Hand-written SVGs, per-figure screenshots | 3% against 0.25% per figure | T5 |
| Reading `PLAN.md`, `SPEC.md`, other chunks in drafting sessions | Large fixed cost per session | 30-line BRIEF.md (T1) |
| Typing prompts into web UIs of other models | Loses text and costs more than drawing or writing directly (PLAYBOOK.md) | Scripts and the API route only |
| Sub-agents writing one huge file | One stalled; chunk agents cost 140-230k tokens each (baeng101 RESUME.md) | One lecture per fresh main session; write in 2-3 parts |
| Building Pro models, redrawn figures, battles for every subject up front | Paid for before any sale | Lite tier first (`new-subjects/PIPELINE.md`) |
| Re-running `qa.js` after every small edit | Slow and holds port 4183 (lesson 33) | `validate.js` per chunk; `qa.js` once per module |

## Measure
Subject total against the 1.3-1.7M token baseline, logged in `new-subjects/SUBJECTS.md`.

## Quality guard
None of these removes a check that caught a real error: coverage, validate, MCQ cross-answer and the phone
preview all stay.

## Done when
These rules are in `new-subjects/PIPELINE.md` and the prompts (they are), and one subject has been built under them.
