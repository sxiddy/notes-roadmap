Status: TODO

# N6 Share a quiz or battle score to WhatsApp with the invite link
Value medium · Cost small (about 3%) · Money/accounts: reads the referral token (no writes) · Order: 6 · Depends on: S02, S03 first

## Goal and benefit
After a quiz, mock test or battle: "I scored 8/10 on Water Pollution. Beat me:" plus the student's own store
invite link. Free growth through the referral program that already exists.

## Design
- The site already knows the store address (`storeUrl` in `/api/session`). The invite token lives on the store
  (`POST /api/offers { action: 'ref-me' }`, needs the store cookie), so the site cannot read it directly.
  Simplest: link to `<storeUrl>/?subject=<CODE>&share=1`; the store page attaches the signed-in student's token
  and opens the share sheet. `app/js/notix-extras.js` already talks to the store: read it first.
- Share text through `navigator.share` with a `https://wa.me/?text=` fallback. Optional: draw the score on a
  canvas for an image card.

## Files
`app/js/app.js` or `app/js/notix-extras.js` (result screens), store `src/scripts/core.js` (handle `share=1`).

## Acceptance
Finish a quiz: Share opens WhatsApp with the score and a link; opening the link signed out lands on the store with
the subject preselected and `?ref=` set. No token appears in any site API answer. `npm test` in the store still green.

## Done when
Works on phone, committed in sites and store.
