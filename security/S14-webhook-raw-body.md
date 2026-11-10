Status: TODO

# S14 Check that the Razorpay webhook signature can be verified on Vercel (Unverified)

> **OWNER APPROVAL REQUIRED: touches payments/referrals/admin/access.** Show the diff, wait for a yes, then commit. No push.

## Problem (not proven: this item starts with a test)
The webhook signature must be computed over the exact raw request body. The handler reads the stream itself, but
Vercel's Node helpers may already have read and parsed the body. If so the raw text is empty, every webhook fails
the signature check, and only the checkout callback and "Check my payment" unlock students. Refund webhooks would
never arrive either.

## Evidence
- `vit-notes-store/api/verify.js:12-17` and `:39-41`; the same `rawBody` in each site's `api/verify.js:14-19`.
- `typeof req.body === 'string'` is false for a JSON request when the helper has parsed it to an object.

## Steps
1. Owner: in the Razorpay dashboard open Webhooks and look at recent deliveries for the store URL. All 200 means
   nothing to do: set Status to DONE with that note. 400 "Bad signature." means the problem is real.
2. If real, pick one:
   a. Export `config = { api: { bodyParser: false } }` is a Next.js option and does NOT apply here. For plain
      Vercel functions set the env var `NODEJS_HELPERS=0` for the project (owner step), which turns off body
      parsing for every function; then confirm `body()` in `_lib.js` still parses JSON itself (it does: it reads
      the stream when `req.body` is undefined).
   b. Or keep helpers and rebuild the raw text only when it is safe: not recommended, because re-serialising JSON
      does not reproduce the signed bytes.
3. Send a test webhook from the dashboard and confirm 200.

## Files
None if step 1 is clean. Otherwise Vercel env (owner) and possibly `api/login.js` form parsing (check that the
Google form POST still works with helpers off; it reads the stream itself when `req.body` is not set).

## Prove it
Razorpay dashboard shows 200 for a test `payment.captured` and a test `refund.processed`; a real Rs 1 code purchase
(owner) unlocks with the tab closed before the callback.

## Rollback
Remove the env var and redeploy.

## Done when
Deliveries are 200, or the finding is closed as "not a problem" with the date.
