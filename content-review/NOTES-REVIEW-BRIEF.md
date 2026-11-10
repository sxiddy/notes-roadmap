# Notes-site content review: shared brief (read fully before starting)

Owner request (11-11-2026): go through EVERY note on a subject site and make sure (1) every concept in the course material is
covered, (2) each note helps the reader understand faster, (3) concepts that past papers stress get more attention. Fix gaps by
adding or rewriting notes, MCQs, flashcards, figures or (only where it truly helps understanding) interactive models. Recommend
extra features and, if small and safe, build them.

## Read first
1. `Projects/NOTES-SITES-LESSONS.md` (every mistake so far; do not repeat them) and the site's `SPEC.md` (writing style) and
   `ADDING_A_MODULE.md`. In evs-notes also `PLAN.md` "State". In baeng101-notes also `src/RESUME.md`.
2. The audit findings the main session passes you (if any).

## Hard rules
- Facts come only from the course material in `src/` (PDF/PPT text, `src/text`, `src/drive-text`) and the past papers. Never invent a
  fact, number, name or answer. If the source contradicts itself, keep the source value and explain it in a `.note heads-up`.
- Short but never drop a topic or fact: after changing a module, run the coverage check (every number and named term in the source
  text for that module appears in the notes). Keep the existing writing style, tab structure and ids; do not rename ids.
- Do NOT touch payments, sign-in, referrals, admin, pricing, env vars or Vercel. No push, no deploy, no `vercel`, no `git push`.
  Commit locally only, one commit per module, with a clear message. Never print secrets.
- Usage: call `get_usage` before every step and stop BEFORE the 5-hour limit reaches 95% (the figure is account-wide and other
  sessions move it). Leave a State note first. One agent at a time; never Haiku.
- Work one module (or one lecture) at a time: read it, review, fix, validate, build, QA, commit, update `REVIEW_STATE.md`
  in the repo root (create it), then move on. Context fills fast; a compaction must never lose unwritten work.
- Verify with real output before saying anything is done: `node tools/validate.js --all` (0 errors), `node tools/build.js`,
  and `node tools/qa.js` (it holds port 4183; if it cannot run on this PC, say so and use the next best check). Use
  `PYTHONIOENCODING=utf-8` for Python. Use Edit/Write (not shell heredocs) for code with backslashes. Do not put apostrophes
  inside single-quoted captions. Rotate MCQ options so answers spread across A-D.

## What "review a note" means
For every topic and tab, ask:
- Is every concept in the source for this topic present, and is anything wrong or ambiguous? (Check numbers, units, definitions.)
- Does it lead with the concept in plain words, then the detail, then an exam hook? Is it faster to read than the slide? Add a "why"
  or a worked example where a student would get stuck; cut padding; keep tabs short.
- Is there a diagram, table, mnemonic or tiny worked problem that would make it click faster? Add one (figures follow
  `tools/figs/PLAYBOOK.md`; interactive models only if the notes hold the numbers and it really aids understanding).
- Do the flashcards and MCQs for the topic cover its key facts, include a few harder multi-step or scenario questions, and match
  how the exam asks?

## Past papers
- Where papers exist (BACHY105: papers feature; BAEEE101: `src/papers-dl` and the papers data; BAENG101: 12 papers in
  `src/papers-dl`; EVS has none, its exams are online): map every paper question to a topic, count how often each concept is
  asked, and list concepts that are asked often but are thin in the notes. Strengthen those notes first, add paper-style MCQs
  for them, and keep the existing "asked in" badge conventions. Put the mapping table in `REVIEW_STATE.md`.

## Features
List recommended features with cost and value in `REVIEW_STATE.md` under "Feature proposals". Build only small, safe, fully
verified ones (e.g. a study aid inside notes); anything touching accounts, money or shared infrastructure is a proposal only.

## Report (final message, under 500 words)
Per module: what was missing or weak, what you changed (counts of notes, MCQs, flashcards, figures, models), what you verified
with which command, what you could not verify, and the top feature proposals. State the final usage figure.
