Status: TEMPLATE (copy into the subject folder as CHECKLIST.md and tick there)

# Per-subject checklist: from source material to launch

OWNER = only the owner can do it. Everything else a session can do.

## Source
- [ ] OWNER: put lecture PDFs/PPTX in `src/pdfs/`, past papers in `src/papers/`; say which weeks are in the exam.
- [ ] OWNER: row filled in `SUBJECTS.md` (code, name, tier, demand signals).
- [ ] Confirm the course code and official name; choose the Redis prefix (`<code lowercase>:`).

## Build (PIPELINE.md)
- [ ] Stage 0 shell copy; leftover-word grep is clean; `git init`, first commit.
- [ ] Stage 1 extract + clean; contact sheets skimmed; image-only tables transcribed.
- [ ] Stage 2 outline approved (OWNER glance: are any topics missing?).
- [ ] Stage 3 every lecture drafted; validate + coverage pass per chunk.
- [ ] Stage 4 flashcards, summaries, questions; MCQ check; figures cropped and looked at.
- [ ] Stage 5 polish; `validate.js --all` 0 errors; `build.js`; `qa.js` and `qa.js --free` clean.
- [ ] `api/_access.json` lists Pro files as intended; `"open": false`; price fallback equals the store list price.

## Infrastructure (lessons 24-26)
- [ ] OWNER: create the private GitHub repo under `sxiddy`; say "push".
- [ ] Create the Vercel project and `vercel git connect` (OWNER approves prompts).
- [ ] Connect the SHARED Upstash Redis to the project (`vercel integration resource connect ...`); do not create
      a new database. Prefix must be set in `api/_store.js` BEFORE connecting.
- [ ] Env vars copied from an existing site by script without printing: `STORE_URL`, `STORE_SECRET`,
      `GOOGLE_CLIENT_ID`, `ADMIN_KEY`, `REVIEW_CODE`, plus a NEW `SESSION_SECRET`. (After S04: its own `ADMIN_KEY`.)
      Read `STORE_URL`, `STORE_SECRET`, `ADMIN_KEY` and `RAZORPAY_KEY_ID` from the STORE project (`getnotespro`), never
      from another subject site: values marked "sensitive" read back empty, and a copied empty/wrong `STORE_SECRET` is
      what kept EVS buyers locked out (lesson 38). A script must stop if any value it reads is empty.
- [ ] OWNER: add the site origin and `/api/login` redirect to the Google OAuth client.
- [ ] Deploy; on `/admin` set mode paid and sign-in on; `launch-check.js` passes.

## Store listing (only after the site and Pro work: lesson 23)
- [ ] `vit-notes-store/api/_lib.js` `PRODUCTS`: add the code (name, tag, site, price).
- [ ] `src/data/subjects.mjs` and the blurb; OG image (`npm run og`).
- [ ] `npm test`, `npm run build`; OWNER says "push"; push `redesign` and `main` as in lesson 22.
- [ ] Each site's `/api/session` shows the store price (lesson 26).
- [ ] OWNER: one real purchase with a 99% code, confirm Pro unlocks on the site, then refund it.
- [ ] The subject's `/admin` shows that purchase under payments (no "Not allowed." store error), and
      `notix-notes.vercel.app/api/cron?job=check` (header `x-admin-key`) lists no problem for the new code.

## After launch
- [ ] Announcement on the store (`/admin` Updates); mark the request "done" if it came from Requests.
- [ ] Log tokens and % of limit in `SUBJECTS.md`; append new mistakes to `NOTES-SITES-LESSONS.md`.
- [ ] At 30 sales: schedule the Full tier (redrawn figures, models).

## Done when
Every box is ticked and the subject row in SUBJECTS.md says LIVE.
