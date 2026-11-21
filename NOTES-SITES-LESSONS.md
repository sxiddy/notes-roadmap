# Notes sites: lessons learned (read before starting any subject site)

Every mistake made while building a notes site goes here, with the fix, so the next subject goes smoothly.
Add to it the moment a mistake happens. Newest at the bottom.

## Setup (copying the BACHY105 shell)
1. **`mv` overwrote a file silently.** Moving `app-src/*` into the project root replaced the existing `README.md`.
   Fix: list both folders first and move with `mv -n` (no overwrite), or rename clashes on purpose.
2. **Hidden files are left behind.** `mv app-src/*` skips `.claude/` and `.gitignore`. Fix: move dotfiles too and
   merge `.claude/launch.json` by hand.
3. **New repo has no git identity.** The first commit failed. Fix: copy `user.name`/`user.email` from
   `bachy105-notes` (`git config user.name Siddharth-uwu`, the GitHub noreply email) right after `git init`.
4. **`tools/build.js` needs a `viz-paid/` folder** even when empty. Fix: `mkdir viz-paid && touch viz-paid/.gitkeep`
   during setup.
5. **The chemistry shell has subject-specific leftovers** in `app/js/app.js` (home search words, syllabus map,
   formula sheet, lab bench, papers) and in `index.html` meta and `SPEC.md`. A string rename does not catch them.
   Fix: grep for the old subject's topic words, not just its code and name.
6. **Generated files** (`api/_access.json`, `_battle.json`, `_content.json`) come from the old subject. Delete them
   and let `build.js` regenerate.

## Previewing
7. **`preview_start` reads `Projects/.claude/launch.json`**, not the subject folder's. Fix: add an entry there that
   does `process.chdir("<folder>")` and `require(process.cwd()+"/tools/serve.js")` (a relative require fails).
8. **The browser pane may refuse localhost URLs.** Fix: rely on `node tools/qa.js` and `qa.js --free` (headless,
   every route and tab), and ask the owner to allow the site if a visual check is really needed.

## Source material
9. **Some tables exist only as images** (EVS W1S3 atmosphere summary table was missing from the extracted text).
   Fix: always skim the page renders as contact sheets and transcribe image-only tables and diagram labels.
10. **Lecture PDFs contradict themselves** (layer heights, temperatures, "two steps" then four, wrong act names).
    Fix: keep the PDF's value in the text and explain it in a `.note heads-up`.
11. **Most pictures in lecture-note PDFs are stock photos.** Only real diagrams, maps, charts and labelled figures
    become figures; skip decoration.

## Writing
12. **Owner rule: short, but never drop a topic or fact.** After writing a module, run the coverage script
    (every number and named term in `src/text/<m>/*.txt` must appear in the notes) before calling it done.
13. **Python on Windows crashes printing Unicode** (₂, α). Fix: run with `PYTHONIOENCODING=utf-8`.
14. **Context fills fast** (Module 1 of EVS, 66 pages, took the session from 20% to 75%). Fix: work one lecture
    (or two short ones) per chunk: read → write chunk → validate → commit, then move on, so a compaction never
    loses unwritten work. Update PLAN.md State after each chunk.
15. **MCQ files can be compact**: a row array mapped into `QUESTIONS({...})` (see EVS `content/q-M1.js`) saves a
    lot of tokens versus writing every field by hand, and still passes `validate.js`.

## Figures
16. **Crops estimated from contact sheets catch stray page text** (9 of 25 in EVS Module 1). Fix: inset each crop
    about 1.5% from the nearest text line, then always tile the `tmp/preview` crops into one contact sheet and look
    before committing.

## FreeLLMAPI
17. **The dashboard API needs the Bearer token from localStorage**, not cookies (`fetch('/api/fallback')` gave 401).
    Fix: read the token key from `localStorage` inside the page and send `Authorization: Bearer ...`; never print it.
18. **`setx` does not reach already-running shells.** After the owner runs `setx FREELLM_KEY ...`, read it with
    `[Environment]::GetEnvironmentVariable('FREELLM_KEY','User')` and pass it into the command.
19. **The unified key is under Keys → "Unified API key" tab** (http://localhost:5173/keys), not on the Providers list.

## Pricing
20. **Owner decisions made in other sessions don't reach this one** (the ₹20 → ₹10 discount was set in the
    "Subject notes pricing and November discount" session, so the EVS shell still had the ₹50 default). Fix: when setting
    up a subject, search other sessions' titles for "price"/"discount" and set the code default (`PRICE_PAISE` in
    `api/_lib.js`, `PRICE_RS ||` fallbacks in `app/js/app.js`). The sale price itself is set on `/admin` after deploy.

## Launching a subject (first done for EVS, 10-11-2026)
21. **Store money tests hardcoded the sale price** (₹10). Changing the sale broke 17 tests. Fix (done): `SALE_PCT` env
    override in `vit-notes-store/api/_lib.js`; the tests pin 50. Change the live sale with the `pct` in `SALE`.
22. **`main` and `redesign` in vit-notes-store had diverged** (the same admin commit twice). Merge `origin/main` into
    `redesign` (keep ours on conflicts), check `git diff <before> HEAD` is empty, then push `HEAD:main` and `redesign`.
23. **Don't list a subject on the store before its site and Pro exist** (students could pay for nothing). Keep it off
    `PRODUCTS` + `src/data/subjects.mjs` until launch (EVS: revert store commit f3e47f5 to list it).
24. **A new subject's Vercel project needs:** its own Upstash Redis (`vercel integration add upstash/upstash-kv` needs the
    owner to finish a browser checkout and to approve creating it), env vars copied from bachy105 by script without printing
    (STORE_URL, STORE_SECRET, GOOGLE_CLIENT_ID, ADMIN_KEY, REVIEW_CODE) plus a fresh SESSION_SECRET, `vercel git connect`,
    and the owner adding the site origin + `/api/login` redirect on the Google OAuth client.
25. **Correction to 24: subjects SHARE one Upstash Redis** (`upstash-kv-carmine-jacket`); don't create a new one (no free
    plan is offered). Each site prefixes its keys in `api/_store.js` (`STORE_PREFIX`, e.g. `bachy101:`). The chemistry
    shell has NO prefix, so copy `_store.js` from baeee101 and change the prefix BEFORE connecting, then
    `vercel integration resource connect upstash-kv-carmine-jacket <project>` and `vercel redeploy <url> --target production`.
26. **Subject sites showed their own stale price** (₹10/₹9) after the store price changed. Fixed 10-11-2026: `withStorePrice()`
    in each site's `api/_lib.js` reads price + MRP from the store's `/api/session` (5-min cache). New subjects get it by
    copying `_lib.js` from any linked site. After a store price change, check every subject's `/api/session` price.
27. **Patch scripts that use `String.replace(anchor, text)`** treat `$` patterns in `text` specially and can eat
    backslashes; use a function replacer `t.replace(a, () => text)` and `require()` the patched file to test it.
28. **Chemistry (bachy105) `tools/qa.js` can't run on this PC** (MODULE_NOT_FOUND, missing package); its build also
    rewrites 6 generated files that are out of date. Don't commit those by accident: `git add` only what you changed.

29. (10-11-2026) Figure `crop` in the notes sites is [x, y, width, height], not [x1, y1, x2, y2]. I wrote end coordinates and got clipped figures; check tools/fig.js before guessing.
30. (10-11-2026) The quiz app does not shuffle options. A hand-written bank drifted to 32 of 54 answers on B. Rotate options in the q-file map, or check the answer spread before committing.
31. (10-11-2026) Sub-agent audits flag figure-only facts as "unverifiable" when the text extraction is lossy. Check the page PNG before "fixing", and tell audit agents to open the page images first.
32. (10-11-2026) Topics written per chunk can span lectures (EVS Week 1 mixes W1S5 and the energy-flow lecture), so a
    "lecture" grouping cannot rely on file order. Fix: `lectures` (n, title, topic ids) in modules.json; app.js sorts
    each week's topics to that order and rebuilds TOPICS/idx so Prev/Next follow it.
33. (10-11-2026) `tools/qa.js` holds port 4183 while it runs, so `preview_start "evs-notes-app"` fails until it ends.
34. (10-11-2026) Figure generators: `R(x,y,w,h,cls,extra)` already writes `class=`, so passing `class="..."` in `extra` makes a duplicate attribute and the SVG fails to load. Validate every generated file with an XML parser before previewing.
35. (10-11-2026) `tools/figs/todo.js` copied from another site matched nothing (EVS figure entries are `{ id, src, crop, caption, alt }`). Check its regex against this site's chunk format before trusting a count.
36. (10-11-2026) Number circles filled `dfw` (the main text colour) hide white text in dark mode. Use an accent fill with fill-opacity and `dt` text.
37. (10-11-2026) Headless Edge needs a second try for the first screenshot of a session (file missing after the first run); rerun the same command once.

38. **EVS (bachy101) had a wrong STORE_SECRET** (fixed 18-11-2026), so store buyers never got Pro and were unlocked by hand.
    After launching a subject, call its `/api/stats` with the admin key and check `payments.notixError` is null. Razorpay
    keys and STORE_SECRET are "sensitive" (unreadable) on the subject sites: read STORE_SECRET from the store project (getnotespro), paste Razorpay keys by hand.
    The store's daily `cron?job=check` now messages Telegram if any subject cannot read its store sales.
39. (18-11-2026) **baphy105 was NOT git-connected on Vercel** (fixed: connected). Each site's /api/stats now shows deploy.git; the daily check flags false. Never `vercel deploy --prod` a subject; deploy by push. Old note:
    baphy105-notes, or run `vercel git connect` there. Notix admin now has: alert bar (daily check, incl. paid-but-locked
    buyers), Inbox for every site's messages (reply/fix from Notix), sites at a glance, recent activity; site messages ping Telegram.
