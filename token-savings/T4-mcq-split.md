Status: TODO

# T4 MCQs: strongest model writes stem and key, cheaper model drafts the rest, scripts check
Estimated saving: about 40% of the question step (estimate)

## Waste today
The strongest model writes every field of every question. Hand-written banks also drifted: 32 of 54 answers were
option B (lesson 30).

## Change
1. Strongest model writes, per question, one compact row: id, topic, type, level, stem, correct answer, the source
   line it rests on. Use the row-array format (lesson 15, see `evs-notes/content/q-M1.js`).
2. Mid-tier model adds three wrong options and the explanation, given the topic's notes. PROPOSAL.
3. Scripts:
   - `tools/mcq-rotate.js`: place the correct option so A-D are evenly spread (the app does not shuffle).
   - `tools/mcq-dump.js` + `tools/mcq-check.py` (exist in evs-notes): two free models answer every question from
     the notes alone; only disagreements are shown. Result so far: 152 of 269 EVS questions checked, 0
     disagreements; single-model routes returned 429, only the router's `fusion` model answered.
   - new check: each wrong option must NOT appear as a true statement in the notes (substring test on key terms).
4. The strongest model reviews only flagged questions.

## Measure
Tokens for the question step per module before and after; number of flagged questions; answer spread.

## Quality guard
Any question two checkers answer differently from the key is rewritten or dropped. Ambiguous distractors are the
known risk: sample 10 per module by hand.

## Done when
One module's bank built this way, spread within 20-30% per letter, numbers logged.
