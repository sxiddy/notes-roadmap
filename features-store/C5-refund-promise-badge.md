Status: TODO

# C5 Refund promise and trust row near the Pay button
Value medium · Cost small (about 1%) · Payment code: not touched (text only) · Order: 5

## Buyer benefit
The last doubt before paying Rs 20 to an unknown site is "what if it is bad". Answer it where the button is.

## Design
- One line under the Pay button in the cart and on subject pages: "UPI through Razorpay · Not happy? Refund
  within <N> days, see Refund policy" linking to `/refund`.
- The text MUST match the real policy in `src/data/legal.json`. Read it first; if the policy has no such promise,
  ask the owner whether to add one. Do not invent a guarantee.
- Add "Made by a VIT student" and the count from C2 when available.

## Files
`src/components/Price.astro` (or wherever the Pay button is rendered: search `src/` for the button text),
`src/scripts/core.js` if the cart is drawn by script.

## Owner input
The refund window in days, or confirmation of the existing wording.

## Acceptance
The line shows at phone width without wrapping badly, links to `/refund`, and says nothing the policy page does not.

## Done when
Committed after the owner confirmed the wording.
