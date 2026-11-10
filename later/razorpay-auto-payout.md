Status: LATER (plan only; do NOT build until the owner says so)

# Automatic referral payouts through RazorpayX

> **OWNER APPROVAL REQUIRED: touches payments/referrals/admin/access.** This moves real money out automatically.

## Today
A student requests a withdrawal (`vit-notes-store/api/_referrals.js withdraw`), it waits in `ref:wdq`, the owner
pays by UPI by hand and clicks "paid" (`markPaid`). Safe, but manual.

## Facts to confirm before any code (owner + Razorpay docs/support)
- RazorpayX payouts are paid from a RazorpayX balance, NOT from the unsettled payment-gateway balance. Money must
  be in the X account first.
- It needs a RazorpayX account (current account or the X Lite wallet, whichever Razorpay offers this business)
  with payouts activated, and its own API access. KYC may be required.
- Fees per UPI payout and any minimum must be checked: a fee of a few rupees on a Rs 15 payout may make this not
  worth doing. If fees are over about 10% of the payout, keep the manual flow or raise the minimum.
- UPI payout needs a contact and a fund account (VPA) created first; VPA validation may cost extra.

## Design (only if the numbers work)
1. New env vars for the X account id and keys (names only in docs; values set by the owner in Vercel).
2. `payout(id)`: create contact + fund account for the UPI ID, then a payout with mode UPI, purpose "cashback",
   and an idempotency key = the withdrawal id (`wd_...`) so a retry can never pay twice.
3. Status by webhook (`payout.processed`, `payout.failed`, `payout.reversed`), signature-verified like
   `api/verify.js`. Withdrawal states: pending, sent, paid, failed.
4. Safeguards, all required:
   - kill switch: config flag `payouts=off` (default off) checked on every call;
   - caps: per withdrawal, per student per month, and a daily total; anything above goes to manual approval;
   - manual approval above a cap, and for any student flagged by S03 (overdrawn);
   - refund hold: only cash older than the S03 hold period is payable;
   - failure or reversal restores the balance (delete the `ref:wd:<email>` entry, keep a log);
   - low X balance: stop and alert the owner (Telegram alert exists in `api/_notify.js`);
   - every payout logged in a hash `payouts` with the Razorpay payout id.
5. Vercel Hobby function limit (12): fold into `api/offers.js`.

## Depends on
S02, S03, S04 done and live for a month without incident.

## Tests
Fake RazorpayX answers: retry with the same id pays once; failed payout restores balance; over-cap goes to
manual; kill switch blocks everything; webhook with a bad signature is refused.

## Done when
Not applicable until promoted to TODO by the owner.
