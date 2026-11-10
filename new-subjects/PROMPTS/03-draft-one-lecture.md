# Prompt 03: Draft ONE lecture (fresh session each time; strongest model)

```
Subject [CODE] in C:\Users\Siddharth D\Projects\[folder]. Check usage first; stop before 95%.
Read ONLY these three things:
1. BRIEF.md
2. src/clean/[module]/[lecture].txt
3. the entry for lecture [n] in tmp/outline-[module].json
Do not open PLAN.md, SPEC.md, other chunks or the app code.

Write content/chunk-[X].js for the topics of this lecture, in the CHUNK({...}) format shown in BRIEF.md.
- Every fact, number, name and formula in the lecture text must appear. Nothing that is not in the text.
- Short tabs, plain words, <b> for the key term on first use. One-liners per topic.
- If the source contradicts itself, keep its value and add a `.note heads-up` explaining it.
- Figure entries: { id, src, crop: [x, y, width, height], caption, alt } for the outline's figures; captions
  without apostrophes.
- Do not write flashcards, questions or summaries here.
Write the file in two or three parts if it is long.

Then run and fix until clean:
  node tools/validate.js content/chunk-[X].js
  node ../notes-tools/coverage.js . content/chunk-[X].js
Commit ("[module] lecture [n]: chunk [X]"). Add one line to STATE.md: lecture done, pages, usage before/after.
Stop. Do not start the next lecture in this session.
```

Verification: `validate.js` and `coverage.js` (scripts). Cost target: 0.07% of a 5-hour limit per slide page.
