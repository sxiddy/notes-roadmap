Status: TODO

# N3 Spaced repetition for flashcards
Value high · Cost medium (about 8%) · Money/accounts: no (flashcards are already Pro) · Order: 3 · Depends on: nothing

## Goal and benefit
"Due today: 23 cards." Cards the student knows come back after 1, 3, 7, 16, 35 days; missed cards come back tomorrow.
Makes Pro worth keeping open all semester.

## Design
- Flashcards come from `protected/data/pro-<chunk>.js` (`PRO_CHUNK({ topics: { <topicId>: { flashcards: [...] } } })`).
  Card id = topic id + index.
- Per card keep `{ box: 0-5, due: 'YYYY-MM-DD' }` in the synced progress object. Know: box+1; Again: box 0, due
  tomorrow. Size check: 600 cards x about 30 bytes fits the 300 KB sync cap (`api/sync.js:10`).
- UI: a "Due today" button on the home page opening the existing flashcard view with a mixed-topic deck, and two
  buttons (Again / Know) in place of plain next.

## Files
`app/js/app.js` (flashcard view, home), `app/css/app.css`.

## Acceptance
Mark 5 cards Know and 2 Again; set the device date one day ahead: 2 due. Three days ahead: 7 due. Non-Pro users see
the existing lock, unchanged. `node tools/qa.js` clean.

## Done when
Works with sync across two browsers, committed per site.
