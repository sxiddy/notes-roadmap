# Prompt 04: Flashcards, summary and question bank for one module

## 04a Derive (mid-tier model; PROPOSAL, see token-savings/T3)
```
Subject [CODE], module [m]. Read only BRIEF.md and content/chunk-[X].js (one chunk per session).
Add 4-8 flashcards per topic ({ front, back }, back under 30 words) to that chunk in the existing field, and add
this chunk's big ideas to content/summary-[m].js (SUMMARY({...}) format in BRIEF.md).
Use only facts already in the chunk's notes. Do not change any notes text.
Run: node tools/validate.js content/chunk-[X].js ; node ../notes-tools/derived-check.js . [m]
Fix until clean, commit, add a line to STATE.md.
```

## 04b Question stems (strongest model)
```
Subject [CODE], module [m]. Read only BRIEF.md and the chunks of this module.
Write content/q-[M].js in the compact row-array format (see BRIEF.md): at least 5 scenario MCQs per topic and
2-3 open questions. For each MCQ give: id, topic, type, level, stem, the correct answer, and the tab it rests on.
Leave wrong options and explanations empty. Every answer must be stated in the notes.
Run node tools/validate.js content/q-[M].js (expect only "missing options" errors). Commit.
```

## 04c Options, explanations, checks (mid-tier model + scripts; PROPOSAL, see T4)
```
Subject [CODE], module [m]. Read content/q-[M].js and the module's chunks.
For each MCQ add three wrong options (plausible, same length and style as the right one, each clearly false
according to the notes) and a two-sentence explanation naming the tab. Do not change stems or correct answers.
Run: node ../notes-tools/mcq-rotate.js content/q-[M].js ; node tools/validate.js content/q-[M].js
Then: node tools/mcq-dump.js ... ; python -I tools/mcq-check.py tmp/mcq.json tmp/mcq-check.json
(the free router must be running; if it answers 429 for everything, set MCQ_MODELS=fusion; if still failing,
skip and list the unchecked ids in STATE.md).
List every question where a checker disagrees with the key. Do not edit them; the next session reviews them.
```

Verification: `derived-check.js`, `validate.js`, answer-letter spread, two free-model answers; the strongest
model reviews flagged items and 10 random questions per module.
