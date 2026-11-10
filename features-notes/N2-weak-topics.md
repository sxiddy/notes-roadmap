Status: TODO

# N2 "Weak topics" list from quiz mistakes
Value high · Cost small (about 4%) · Money/accounts: no · Order: 2 · Depends on: nothing

## Goal and benefit
After a few quizzes the student sees the five topics they get wrong most, each with "re-read" and "retry the
questions I missed" buttons. Turns the quiz into a study guide.

## Design
- Every question has a `topic` field (`content/q-*.js`). On each first answer record `{ right, wrong }` per topic
  and the ids missed, inside the synced progress object (check first what the quiz already stores; reuse it).
- Weak score = wrong / (right + wrong), shown only for topics with at least 3 answers.
- "Retry missed" opens the existing quiz view filtered to the missed ids. Free users: questions are free; answers
  and explanations stay Pro as today (no gating change).

## Files
`app/js/app.js` (quiz answer handler, home or progress page), `app/css/app.css`.

## Acceptance
Answer 6 questions on one topic with 4 wrong: the topic tops the list at 67%. Retry shows exactly those 4. A
topic with 2 answers is not listed. Data survives reload and sync. `node tools/qa.js` clean.

## Done when
Live in evs-notes locally, copied to the others, committed.
