Status: TODO

# C6 Referral progress bar
Value medium · Cost small (about 2%) · Payment code: reads referral numbers only · Order: 6 · Depends on: S02, S03

## Buyer benefit
"2 of 5 friends, Rs 9 more to withdraw Rs 15" is far more motivating than a bare link.

## Design
- `POST /api/offers { action: 'ref-me' }` already returns `friends`, `per`, `min`, `balance`, `withdrawals`
  (`api/offers.js:60-66`). Draw a 5-step bar from `balance / min`, the share buttons (WhatsApp exists in
  `src/scripts/core.js`), and after S03 the "available on <date>" line.
- Show the invite panel right after a successful payment too ("Get Rs 3 back for each friend").

## Files
`src/scripts/core.js` (invite panel, payment success view), `src/styles/global.css`.

## Acceptance
With 2 friends the bar shows 2 of 5 and Rs 6; at 5 the Withdraw button is enabled; signed out shows a sign-in
prompt. `npm test` unchanged and green.

## Done when
Committed.
