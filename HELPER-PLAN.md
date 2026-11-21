# Helping Notix: a work plan for a volunteer with spare Claude usage

Thank you for giving part of your Claude usage to this. This file assumes you know nothing about me or the projects, so it explains everything you need. Read **Part A** once (about 10 minutes), set up with **Part B**, then work through **Part C** from the top. Part C is sorted by priority: first what is broken or losing money, then the big, usage-heavy jobs I can't afford to do myself, then the nice-to-haves. Most tasks have a prompt you can paste straight into Claude Code.

---

## Part A: what this is

### The business
I'm a first-year student at **VIT Vellore**, a large engineering university in India with about 35,000–40,000 students. I run **Notix** (https://notix.in), a small store that sells study-notes websites for first-year courses. Each course has its own site:

| Site (live address) | Course | Repo |
|---|---|---|
| bachy101.notix.in | Environmental Science (EVS), the most complete site and the **reference** for the others | `bachy101-notes` |
| bachy105.notix.in | Engineering Chemistry | `bachy105-notes` |
| baeee101.notix.in | Basic Electrical & Electronics Engineering | `baeee101-notes` |
| baeng101.notix.in | Technical English / communication | `baeng101-notes` |
| baphy105.notix.in | Engineering Physics | `baphy105-notes` |
| notix.in | The store, plus my **admin portal** (payments, visits, reports) | `getnotespro` |
| — | Plans, specs, checklists and lessons learned for all of the above | `notes-roadmap` |

**How students use it:** they sign in with their university Google account (only `@vitstudent.ac.in` addresses are accepted). All notes are free. **Pro** costs about ₹20 (around US$0.25), paid through **Razorpay** (an Indian payment gateway, like Stripe). Pro unlocks extras such as mock tests, quiz battles against other students, and revision tools. A Pro account can be used on 1 phone and 1 laptop.

**What a notes site contains:** each module of the course has topics with notes, tables, diagrams, flashcards and multiple-choice quizzes (MCQs). There are also past exam papers with model answers. The university calls its exams **CAT** (mid-terms) and **FAT** (finals). For some courses, including EVS, the exam is MCQ-only, which is why notes are kept short and keyword-heavy.

### The tech (same on every notes site)
- **Static front end:** `app/` (HTML, `app/js/app.js`, `app/js/gate.js`). Content lives in `content/chunk-*.js`, and `tools/build.js` turns it into the files the browser loads (`protected/data/`, `dist/`).
- **API:** `api/*.js` (login, session, payment, quiz battle, progress). It was written as Vercel-style functions and now runs on a **Cloudflare Worker** (`tools/cf-build.js` + `wrangler`).
- **Database:** **Turso** (hosted SQLite) behind a Redis-like wrapper. Locally, `tools/fakeredis.js` stands in for it.
- **No framework, no TypeScript, Node 22+.**
- The store (`getnotespro`) is an Astro site plus a Cloudflare Worker (`worker/`), with Turso too. It holds the store pages, the admin portal (`public/js/admin-*.js`) and the payment and referral logic.

The five notes sites are near-copies of each other. **A fix to shared code (`api/`, `app/js/`, `tools/`) usually has to be repeated on all five.** EVS is the one to copy from.

### Where the knowledge lives
`notes-roadmap` is the brain. Read these first:
- `README.md` and `HOW-TO-USE.md`
- `MODELS-AND-BUILD-METHODS.md`: how I use Claude on this, and what has gone wrong before
- `NOTES-SITES-LESSONS.md`: numbered lessons learned the hard way. Claude should read it before touching any site.
- Every task file starts with `Status: TODO` and holds a full spec. **Some specs predate the move from Vercel and Upstash to Cloudflare and Turso, so check the current code before trusting a file path or line number in a spec.**

---

## Part B: setup and how to test without my accounts

### Rules
1. **Fork + pull request for everything.** Fork the repo on GitHub, work on a branch in your fork, and open a PR against `sxiddy/<repo>`. The default branches are protected, and you can't deploy anyway: I review, merge, deploy and check live.
2. **You get no secrets, and you don't need any.** Everything below runs locally with fakes. Never ask for or commit keys.
3. **Code first, content second.** You're not a VIT student, so prefer code tasks. Leave rewriting course content to me unless a task says otherwise.
4. **One task = one PR.** A shared change done on all five sites can be one PR per site, or one per task if you script it.
5. **Evidence in every PR:** the check commands you ran and their output, screenshots for UI changes, and a "not tested" list (normally: real Google sign-in, real payment, live Cloudflare).
6. At the end of each task, add a line to `notes-roadmap/HELPER-LOG.md` (a small PR to notes-roadmap, or list it in your code PR and I'll add it): task id, PR link(s), state.
7. **CI safety:** if you add GitHub Actions, use the `pull_request` trigger only, never `pull_request_target`, and no workflow that needs secrets. The repos have no Actions secrets and should keep it that way.

### Run a notes site locally
```bash
git clone https://github.com/<you>/bachy101-notes.git && cd bachy101-notes
npm install
node tools/build.js            # build content
DEV_LOGIN=1 node tools/serve.js   # http://localhost:4173
```
- With no payment keys present, `serve.js` turns on `DEV_BYPASS`, so **Pro features unlock** without paying.
- With `DEV_LOGIN=1`, you can sign in as any made-up student, e.g. `test1@vitstudent.ac.in`. This works only locally and is off in production.
- `PAYWALL=1 node tools/serve.js` forces the paywall, to test the locked state.
- Use two browser profiles with two dev emails to test quiz battles.

### Checks to run before every PR (notes sites)
```bash
node tools/build.js && node tools/validate.js && node tools/qa.js
node tools/coverage.js          # content coverage, if you touched content (EVS only for now)
python tools/mcq-check.py       # quiz answer sanity, if you touched MCQs (EVS only for now)
```
Also open the changed pages at **375 px width** (phone). No sideways scrolling, and no console errors.

### Run the store locally
```bash
git clone https://github.com/<you>/getnotespro.git && cd getnotespro && npm install
npm test                         # existing node --test suite in tests/
# create .dev.vars with obvious fake values (no example file exists yet; task P1-7 adds one)
npm run build && npx wrangler dev   # Astro site + Worker (config in wrangler.jsonc, code in worker/)
```
If a required variable isn't documented, list it in your PR. Task P1-7 fixes that properly.

### Claude Code tips for this repo
- Start each task in a **fresh session** with the prompt given. Long sessions waste usage.
- Useful skills, if you have them: `superpowers-writing-plans`, `superpowers-executing-plans`, `superpowers-verification-before-completion`, `addy-test-driven-development`, `addy-security-and-hardening`, `addy-incremental-implementation`, `anthropics-webapp-testing`. None is required.

---

## Part C: the tasks, in priority order

I went through all ~150 of my Claude sessions and every plan file to build this list. One-off chats (homework, study guides, PDF jobs) were dropped. What's left is sorted into three tiers:

- **Priority 1 (P1): needed now.** It's broken in production, costs real money, or could take the sites down on an exam night. Most items are small but urgent.
- **Priority 2 (P2): needed, and usage-heavy.** These are the big jobs I can't afford to run myself. **This is where your usage helps most.**
- **Priority 3 (P3): good to have.** Do these if budget is left.

Sizes are a share of a **weekly** limit: **S** under 2%, **M** 2–5%, **L** 5–10%. Every task stands on its own, so stop wherever your budget ends.

**Before any task, check whether it's still open.** I keep working too. Search the code, and skip the task (noting it in the log) if it's already done.

### Priority 1: broken, or costing money (~6% total)

| Id | Size | Repo(s) | Task |
|---|---|---|---|
| P1-1 | S | all 5 notes sites | **Wrong price shown.** `app/js/notix-extras.js` says "Pro (₹10)" while the real price is ₹20 (list price ₹30). The fallbacks in `api/_lib.js` (₹10/₹50) are wrong too. Read the price from the same place checkout uses, so the two can never disagree. |
| P1-2 | S | getnotespro | **The EVS store page sells Pro with an empty feature list** (`src/data/subjects.mjs`, the `bachy101` entry, `pro: []`). Fill it from what EVS Pro actually unlocks: Review, Mock test, Mistake notebook, Numbers drill, battles and so on (check `bachy101-notes/app/js`). Then check the other four subjects' lists against their code. |
| P1-3 | S | bachy105-notes | `node tools/validate.js --all` fails with 124 old figure-source errors, so CI would fail from day one. Fix the errors, or add a reviewed allow-list with a comment explaining why. |
| P1-4 | M | all 5 notes sites | **CI on every PR** (it caught nothing when a syntax error shipped to 4 sites at once). Prompt below. |
| P1-5 | M | all 5 notes sites | **Request budget.** The free Cloudflare plan stops at 100,000 requests a day, about 2,400–3,300 students actually studying, and exam nights get close to that. Measure requests per page load, then (a) bundle the figures so each image isn't its own request, (b) slow quiz-battle polling from 1.5 s to 3 s and pause it while the tab is hidden, (c) batch visit tracking, (d) set long cache headers on built files. Report before and after counts. |
| P1-6 | S | getnotespro | Check the usage alert in `api/_notify.js`/`api/cron.js` fires at 70% of the daily **Worker** request limit. Fix it if it doesn't, with a test. |
| P1-7 | M | getnotespro + bachy101-notes | **Payments silently stopped being recorded once** because a site was missing environment variables. Prompt below. |
| P1-8 | S | getnotespro | The admin totals code in `api/admin.js`/`api/cron.js` (per-site Razorpay counts, with a fallback to backup hosts) has no tests. Add tests with mocked fetches, covering a slow site, a down site and a mirror answering instead. |

> **P1-4 prompt:** *"Read README, NOTES-SITES-LESSONS.md and tools/ in this repo. Add .github/workflows/ci.yml (trigger: pull_request only, no secrets) that runs npm ci, node tools/build.js, validate.js, qa.js, and `node --check` on every file in app/js and api. Add a Playwright smoke test: start tools/serve.js with DEV_LOGIN=1, sign in as a dev student, open home, one topic, one quiz (answer a question) and the papers page at 375 px and 1280 px, and fail on any console error. Make it pass locally, then copy it to the other four sites."*
>
> **P1-7 prompt:** *"In getnotespro and bachy101-notes, find every process.env / env.X read. Write ENV.md listing each variable, which site needs it and whether it's secret (names only, never values). Add a startup check that returns a clear 500 'missing config: X' (no values) when a required variable is missing, plus .dev.vars.example with fake values. Add tests for the Razorpay webhook path that prove a payment is logged exactly once (see notes-roadmap/security/S08 and S14)."*

### Priority 2: needed, and usage-heavy (~22% total). This is where you help most.

**2A. Money and security (~6%).** Every file in `notes-roadmap/security/` has the problem, the fix and how to test it. Some predate the move to Cloudflare, so confirm each issue still exists first.

| Id | Size | One line |
|---|---|---|
| S01 | S | Content must be locked, not unlocked, if the config fails to load |
| S14 | S | Verify the Razorpay signature on the raw body |
| S08 | S | A refund must be recorded once |
| S02 | S | Referral credit based on the amount actually paid |
| S03 | M | Referral withdrawals vs later refunds |
| S04 | M | Admin key: rate limit and rotation (constant-time compare already exists on the notes sites; check the store) |
| S05 | M | The 1 phone + 1 laptop rule can be dodged |
| S06, S07, S10, S13 | S–M | Shared-database key prefixes, visit-counter abuse, Gmail-alias gift abuse, error leaks and session expiry |
| P2-0 | S | The store's dev sign-in (`api/login.js`) only checks `!VERCEL`. Also require `!PRODUCTION`, as the notes sites already do, and set `PRODUCTION=1` in the store's `wrangler.jsonc` vars. |

> Prompt per item: *"Read notes-roadmap/security/SXX-*.md and NOTES-SITES-LESSONS.md. Confirm against the current code whether the issue still exists, with file:line evidence. If it does: write a failing test, fix it, make the test pass, and apply the fix to every affected site. PR description: issue, evidence, fix, test output."*

**2B. Bring every site up to EVS (~8%).** Students notice these, and they're the biggest code job.

| Id | Size | Task |
|---|---|---|
| P2-1 | L | **Port the EVS study tools to the other four sites:** Review (spaced repetition with "Knew it / Lucky guess"), Mistake notebook, Mock test (50 MCQs in 30 min), the home card ("exam-ready" score + "Do this next"), Numbers drill, Memory tricks shell, Explain-simply shell. The code is in `bachy101-notes` commit `072f6db`. **Write a port script, don't hand-copy five times.** Subject-specific text (tricks, explanations) stays empty for me to fill. |
| P2-2 | M | **Fast track page shell** for baeee101, baeng101 and baphy105, with the page and the 10-MCQs-per-module mechanism. Copy it from chem (`bachy105-notes` commit `002c73e`). I'll write the fact lists. |
| P2-3 | M | **BAPHY105 audit** (the newest site, cloned from Basic Electrical): find leftover Basic Electrical text in `app/`, `api/` and help text (it still says "Module 2" and "Modules 3 and 4" in places); compare what's free and what's Pro with chem; check every feature the store promises really exists. Spec: `new-subjects/TODO-BATTLE-AND-ACCESS.md` section 0. Signed-out and dev-login checks only. |
| P2-4 | M | **Free quiz battle rule** on all five sites: free users can battle on any one module and pick several topics in it (today the code requires `topics.length === 1`). Spec: `TODO-BATTLE-AND-ACCESS.md` section 2. |
| P2-5 | S | **MCQ answer balance:** in baeee101, 80% of answers are A or B (baeng101 is skewed too). Write a script that reorders options so each letter is the answer ~25% of the time, without changing which answer is correct, and prove it with a before/after count plus a correctness check. Run it on all five sites. |
| P2-6 | M | **Chemistry worked-example checker:** `bachy105-notes/tools/check-numericals.js` recomputes the 63 worked numericals and flags any whose stated answer is off. Pure maths, no course knowledge needed. Report what's wrong in the PR, and I'll fix the content. |

**2C. Admin portal: everything editable (~3%, L).** I want to change any setting from the admin portal without touching code: prices (list price and sale price), the sale end date (the sale currently ends on its own on **1 Dec 2026**), discounts, which subjects are listed, banners, report replies and site links. Keep it tidy and easy to navigate.
> Prompt: *"In getnotespro, list every business value that's hard-coded or env-only (prices PRICE_<CODE>, the sale end date in api/_lib.js, subject list, discounts, copy, links) with file:line. Move them into a settings table in Turso with today's values as defaults, and add an admin 'Settings' page grouped by section, with validation and an audit log of changes. The store and the five notes sites must read prices from it, so the price can never disagree between store, site and checkout again. Test with wrangler dev. Plan first and stop for my OK before building."*

**2D. VIT CGPA calculator: big UI redesign (~5%, L)**, in `sxiddy/vitcgpacalc`.
**What it is:** a free tool (now at markscalc.cv) where VIT students work out their GPA and CGPA (grades S, A, B, C, D, E, F on a 10-point scale, weighted by credits), with a profile, a shareable result card with a QR code, and an admin page.
**Tech:** plain HTML pages (`index.html`, `calc.html`, `cgpa.html`, `profile.html`, `share.html`, ...), one stylesheet `css/style.css`, scripts in `js/`, a small API in `api/` (Turso), a service worker `sw.js`. No framework. Run with `node tools/serve.js`; tests are `node tools/test-calc.js` and `node tools/test-server.js`.
**The ask:** it works but looks generic. Make it the kind of student tool people screenshot and share:
- One clear visual direction (type, colour, spacing, motion) on every page. Dark by default; light theme if it fits.
- **Mobile first:** entering 6–8 courses must be fast with thumbs, with big tap targets, a grade picker instead of typing, and no zooming.
- An instant, satisfying result: an animated GPA/CGPA number and a grade breakdown chart.
- A share card worth posting (`js/card.js`, `share.html`). Check the share card, QR code and SEO tags all use the new address.
- Don't change what's calculated: `tools/test-calc.js` must still pass. Add a CI workflow for both test files.
- Accessibility: contrast, keyboard use, labels.
> Prompt: *"Read every HTML page, css/style.css and js/ui.js in vitcgpacalc. Propose 2–3 distinct visual directions as static mock pages (index + calc at 375 px and 1280 px) and stop for the owner to pick one. Then apply the chosen direction to every page, mobile first, with a grade picker, an animated result and a better share card. Keep all calculation logic and tests passing; screenshot every page at both widths for the PR."*
Skills that suit this: `impeccable`, `frontend-design`, `design-taste-frontend`.

### Priority 3: good to have, if budget is left (~7%)

| Id | Size | Repo | Task |
|---|---|---|---|
| P3-1 | L | notes-tools | **One shared copy of the helper scripts.** Spec: `new-subjects/SCRIPTS-TO-BUILD.md`, with `sync-shell.js` first and its drift report saved as `content-review/SHELL-DRIFT.md`. Makes every future "apply to all five" job cheap. |
| P3-2 | M | roblox-gfx-pipeline | **Film checker:** given a rendered video, report repeated and frozen frames, shot lengths (cut detection), subtitle timing and loudness (LUFS) with ffmpeg, compared with the targets in `docs/SUPERIOR_PLAN.md`. Test it on a generated clip. |
| P3-3 | S–M | roblox-gfx-pipeline | **gate.py false fails:** night-scene brightness defaults fail dark scenes; `head_frac` reads 0.8× the real head; the face-rig plate is counted as a blocker; body support needs about −2 mm ground tolerance. Fix the defaults and add unit tests (there are none for `tools/` yet). |
| P3-4 | S–M | roblox-gfx-pipeline | **Better subtitles:** replace the plain Pillow default with a reusable animated style (bold font, outline/shadow, phone-size check, kept off busy areas). |
| P3-5 | M | roblox-gfx-pipeline | **Zigzag talking mouth:** the face rig should keep an avatar's signature mouth (such as zigzag teeth) while the mouth opens for lip sync. Code only, on a test rig; I'll check it on real avatars. |
| P3-6 | L | drive-reel-mcp | **Editor controls** that are still missing: time chips, per-shot crop, burst/ramp, choosing the song section on the waveform. Read `README.md`, `AGENTS.md`, `HANDOFF.md` first. |
| P3-7 | M | getnotespro | Store growth features from `features-store/`: C1 SEO (sitemap, JSON-LD), C3 free sample topic, C4 sale countdown (pairs with 2C), then C7 bundle, C9 semester pass, C2/C5/C6/C8. |
| P3-8 | M each | notes sites | Features from `features-notes/`: N2 weak topics, N1 exam countdown plan, N5 cram sheet (generated from flashcards), N4 offline install, N7 battle leaderboard, N6 score share card. N3 is probably covered by Review already. |
| P3-9 | S | notes-roadmap | Run `tools/mcq-check.py` on all 269 EVS MCQs (only 152 were checked) and list anything suspicious in a PR. I'll judge the content. |
| P3-10 | L | baeng101-notes | S09: port the English site to the shared shell, using the `sync-shell` report from P3-1. |
| P3-11 | — | notes-roadmap | Token-saving scripts T1–T6 (`token-savings/`). |

**Video pipelines (`minecraft-video-editing`, `roblox-video-editing`): hold.** Both repos are generated from another repo of mine, so a PR to them would be overwritten. I'll tell you if I open that one up.

---

## Part D: budget and how to spend it

| Tier | Share of your ~35% weekly | What it buys |
|---|---|---|
| Priority 1 | ~6% | Nothing broken, no lost payments, no exam-night outage |
| 2A security | ~6% | Real money protected |
| 2B parity | ~8% | Every subject as good as EVS |
| 2C admin settings | ~3% | I stop editing code to change a price |
| 2D CGPA redesign | ~5% | High-traffic free tool, needs the best design model |
| Priority 3 | ~7% | Whatever is left |

If a task runs well past its size, stop, open a draft PR with what's done, and move on.

**How to save usage:**
- **Use the strongest model for design and tricky logic** (CGPA directions, security fixes, the admin settings design). **Use a cheaper model such as Sonnet for mechanical work:** porting to the other four sites, tests from a spec, CI files.
- **Build once on EVS, then port with a script,** not by having Claude rewrite the same change five times.
- **A fresh session per task,** started with its prompt. Long sessions waste usage.
- **Point Claude at the spec and the exact files** rather than letting it explore the whole repo.
- **For L tasks, plan first,** check the plan, then implement. A wrong direction is the biggest waste.
- **Run the quick checks while you work,** and save the full browser run for just before the PR.

---

## Not yours (needs me)
Deploying; the Cloudflare, Razorpay, Turso, Vercel and Google dashboards (rate-limit rules, old-link redirects, the UPI display name, backup hosts, whether to pay for Cloudflare's $5 plan); real sign-in and payment checks on the live sites; writing course content (thin tabs, missing figures, past papers, the English FAT answers, the physics past papers); anything that needs VIT's portal; my avatars, footage and Blender scenes.

## Talking to me
Open a PR and message me the link. If a spec is unclear or seems wrong, ask in the PR rather than guessing. I'm in India (IST, UTC+5:30) and answer when I'm online.
