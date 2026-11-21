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
1. **Fork + pull request for everything.** Fork the repo on GitHub, work on a branch in your fork, and open a PR against `sxiddy/<repo>`. **Base your work on the branch that is live, not the default branch:** `cloudflare` on the five notes sites (it is 3–7 commits ahead of `main`/`master`), `redesign` on `getnotespro`, and the default branch everywhere else. These branches are protected, and you can't deploy anyway: I review, merge, deploy and check live.
2. **Don't trust `Status:` lines in spec files.** Many still say TODO when the work is partly done. Check the code.
3. **You get no secrets, and you don't need any.** Everything below runs locally with fakes. Never ask for or commit keys.
4. **Code first, content second.** You're not a VIT student, so prefer code tasks. Leave rewriting course content to me unless a task says otherwise.
5. **One task = one PR.** A shared change done on all five sites can be one PR per site, or one per task if you script it.
6. **Evidence in every PR:** the check commands you ran and their output, screenshots for UI changes, and a "not tested" list (normally: real Google sign-in, real payment, live Cloudflare).
7. At the end of each task, add a line to `notes-roadmap/HELPER-LOG.md` (a small PR to notes-roadmap, or list it in your code PR and I'll add it): task id, PR link(s), state.
8. **CI safety:** if you add GitHub Actions, use the `pull_request` trigger only, never `pull_request_target`, and no workflow that needs secrets. The repos have no Actions secrets and should keep it that way.

### Run a notes site locally
```bash
git clone -b cloudflare https://github.com/<you>/bachy101-notes.git && cd bachy101-notes
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
git clone -b redesign https://github.com/<you>/getnotespro.git && cd getnotespro && npm install
npm test                         # existing node --test suite in tests/
# create .dev.vars with obvious fake values (no example file exists yet; task P1-7 adds one)
npm run build && npx wrangler dev   # Astro site + Worker (config in wrangler.jsonc, code in worker/)
```
If a required variable isn't documented, list it in your PR. Task P1-7 fixes that properly.

### Claude Code tips for this repo
- Start each task in a **fresh session** with the prompt given. Long sessions waste usage.
- Useful skills, if you have them: `superpowers-writing-plans`, `superpowers-executing-plans`, `superpowers-verification-before-completion`, `addy-test-driven-development`, `addy-security-and-hardening`, `addy-incremental-implementation`, `anthropics-webapp-testing`. None is required.

---

## Part C: what's still pending, in priority order

**Checked on 21 Nov 2026 against the live code** (the `cloudflare` / `redesign` branches and the live sites). Anything already fixed has been taken out. If you find something else already done, skip it and note that in the log.

- **Priority 1 (P1):** wrong on the live site or a risk to payments now. Mostly small.
- **Priority 2 (P2):** needed, and usage-heavy. These are the big jobs I can't afford to run myself, **so this is where your usage helps most.**
- **Priority 3 (P3):** good to have.

Sizes are a share of a **weekly** limit: **S** under 2%, **M** 2–5%, **L** 5–10%. Every task stands on its own.

### Priority 1: wrong on the live site, or a risk to payments (~6%)

| Id | Size | Repo(s) | Task |
|---|---|---|---|
| P1-1 | S | all 5 notes sites | **The banner shows the wrong price.** `app/js/notix-extras.js:100` says "the rest are in Pro (₹10)"; the price is ₹20 (₹30 list). Also, the `PRICE_PAISE` fallback in `api/_lib.js` differs per site (2000 / **5000** / **1000** / **1000** / 2000). Show one price that comes from the same source as checkout, and make a missing price an error instead of a silent fallback. |
| P1-2 | S | getnotespro | **The EVS store page has no "Pro adds" list** (`src/data/subjects.mjs:98`, `pro: []`), so notix.in/bachy101 shows none while the other subjects do. Fill it from what EVS Pro really unlocks (check `bachy101-notes@cloudflare` `app/js`: Review, Mistake notebook, Mock test, Numbers drill, Memory tricks, Fast track, battles…). Use the same style as the bachy105 entry. |
| P1-3 | S | getnotespro + notes sites | **S01 leftovers: content must lock if config fails.** bachy105 `api/_lib.js:104` uses `freeMode = m !== 'paid'` (should be `m === 'free'`). baeee101 and baeng101 have `"open": true` in `api/_mode.json`. Fix the code, and mark the `_mode.json` changes clearly in the PR, since I'll confirm those settings before merging. |
| P1-4 | S | getnotespro | Set `PRODUCTION=1` in the store's `wrangler.jsonc` vars, so the dev sign-in can never be active live. The `!PRODUCTION` check is already in `api/login.js` on `redesign`. |
| P1-5 | M | all 5 notes sites | **CI on every PR.** There are no workflows anywhere. Note that bachy105's `node tools/validate.js --all` currently fails with **124 errors**: fix them, or add a reviewed allow-list, so CI starts green. Prompt below. |
| P1-6 | M | all 5 notes sites | **Request budget.** The free Cloudflare plan stops at 100,000 requests a day, and exam nights get close. Battle polling and visit batching are already done. Still open: (a) **bundle the figures** (`api/img.js` serves each SVG separately, 39–505 per site); (b) long `immutable` cache headers on built static files. Report requests per page load before and after. |
| P1-7 | S | getnotespro | **Usage alert.** `api/cron.js` only alerts on failed health checks. Add a Telegram alert (reuse `api/_notify.js`) at 70% of the daily Worker request limit, with a test. |
| P1-8 | M | getnotespro + bachy101-notes | **Payments silently stopped being recorded once.** Add ENV.md, `.dev.vars.example` and a startup "missing config" check, plus webhook tests. None of these exist yet. Prompt below. |
| P1-9 | S | getnotespro | Tests for the admin totals code (`api/admin.js`/`api/cron.js`: per-site Razorpay counts, falling back to backup hosts) with mocked fetches: a slow site, a down site, a mirror answering. |

> **P1-5 prompt:** *"Read README, NOTES-SITES-LESSONS.md and tools/ in this repo (branch cloudflare). Add .github/workflows/ci.yml (trigger: pull_request only, no secrets) that runs npm ci, node tools/build.js, validate.js, qa.js, and `node --check` on every file in app/js and api. Add a Playwright smoke test: start tools/serve.js with DEV_LOGIN=1, sign in as a dev student, open home, one topic, one quiz (answer a question) and the papers page at 375 px and 1280 px, and fail on any console error. Make it pass locally, then copy it to the other four sites."*
>
> **P1-8 prompt:** *"In getnotespro (branch redesign) and bachy101-notes (branch cloudflare), find every process.env / env.X read. Write ENV.md listing each variable, which site needs it and whether it's secret (names only, never values). Add a startup check that returns a clear 500 'missing config: X' (no values) when a required variable is missing, plus .dev.vars.example with fake values. Add tests for the Razorpay webhook path that prove a payment is logged exactly once."*

### Priority 2: needed, and usage-heavy (~22%). This is where you help most.

**2A. Money and security (~6%).** Specs are in `notes-roadmap/security/`. They predate Cloudflare, so follow the logic, not the old line numbers. S14 is already fixed, and S01 moved to P1-3.

| Id | Size | Still open (checked in code) |
|---|---|---|
| S08 | S | Refunds: `api/verify.js:63` logs a refund once per cart item, each with the full amount. Log a cart refund once, split full refunds by each item's share, skip refunds the admin already logged, and add `tests/refunds.test.js`. |
| S02 | S | Referral credit is still given when a discount code is used (`api/create-order.js:65`), with no minimum amount. Add tests. |
| S03 | M | Withdrawals vs later refunds: there's no "overdrawn" state, hold period or `ref-cancel` yet. |
| S04 | M | Admin key: drop the `?key=` form in `api/cron.js:17`, and lock out after 10 wrong keys per address per 15 minutes on the store and all sites. |
| S05 | M | Device limit: all 5 sites `api/_lib.js` (~315–342) `if (!u \|\| !u.d) return true;`. Allow a cookie with no device id only for 10 minutes after sign-in. |
| S06 | M | bachy105 and the store use the same Turso database with no key prefix. Add `bachy105:` and `store:` prefixes with a dry-run migration and a reverse script. **Prepare it only;** I'll run it in a quiet hour. |
| S07 | S | `api/track.js` on all 5 sites has no rate limit and takes labels from the request. Accept only ids in `_content.json`, take the labels from there, and limit by address. |
| S10 | S | Gift emails accept `+` and `%` aliases. Refuse them, warn about emails that have never signed in, and add an admin "move Pro to another email" action. |
| S13 | M | Cookies last 400 days, and errors show `e.message` plus "Set it in Vercel". Show generic errors, use a 180-day maximum with refresh after 7 days, and a 30-day owner cookie. |

> Prompt per item: *"Read notes-roadmap/security/SXX-*.md and NOTES-SITES-LESSONS.md. Confirm against the live branch whether the issue still exists, with file:line evidence. If it does: write a failing test, fix it, make it pass, apply it to every affected site. PR: issue, evidence, fix, test output."*

**2B. Bring every site up to EVS (~8%).**

| Id | Size | Task |
|---|---|---|
| P2-1 | L | **Port the EVS study tools to bachy105, baeee101, baeng101 and baphy105.** EVS has Review (spaced repetition), Mistake notebook, Memory tricks, the exam-ready home card, Numbers drill, Explain-simply and the new Mock test (50 MCQs in 30 min). The other four have none of these: their "mock" is the old CAT/FAT paper mock, so keep that too. Code reference: `bachy101-notes` commit `072f6db`. **Write a port script, don't hand-copy.** Leave subject-specific text empty for me. |
| P2-2 | M | **Fast track page shell** for baeee101, baeng101 and baphy105 (EVS and chem have it; copy chem's `bachy105-notes` commit `002c73e`). I'll write the fact lists. |
| P2-3 | M | **Physics site (baphy105) clean-up.** It still says "one battle on a single Module 2 topic" (`app.js:2441`) and "plus Modules 3 and 4" (`app.js:2579`), though Modules 1–4 are free. `FREE_TOOL = 'opamp-gain'` is left over from Basic Electrical. Then do the full audit in `new-subjects/TODO-BATTLE-AND-ACCESS.md` section 0. The same leftovers exist elsewhere (S11): "Module 2" wording in every `api/battle.js`, and EVS `FREE_TOOL = 'beer-lambert'`, which is a chemistry tool. |
| P2-4 | M | **Free quiz battle rule** on all 5 sites: today it's `topics.length === 1` (`api/battle.js:34`). Free users should be able to pick any one module and several topics in it. Spec: `TODO-BATTLE-AND-ACCESS.md` section 2. |
| P2-5 | M | **Chemistry worked-example checker:** `bachy105-notes/tools/check-numericals.js` recomputes the 63 worked numericals and flags wrong answers. Pure maths. List the problems in the PR, and I'll fix the content. |

**2C. Admin portal: everything editable (~3%, L).** Prices are env-only (`PRICE_<code>`), the sale end date is hard-coded to **1 Dec 2026** (`api/_lib.js:24`), and the admin shows pricing read-only. I want to change prices, the sale and its end date, discounts, listed subjects, banners and links from the admin portal, kept tidy and easy to navigate.
> Prompt: *"In getnotespro (branch redesign), list every business value that's hard-coded or env-only (prices, sale end date, subject list, discounts, copy, links) with file:line. Move them into a Turso settings table with today's values as defaults, and add an admin 'Settings' page grouped by section, with validation and an audit log. The store and the five notes sites must read prices from it. Test with wrangler dev. Plan first and stop for my OK before building."*

**2D. VIT CGPA calculator: big UI redesign (~5%, L)**, in `sxiddy/vitcgpacalc` (live at markscalc.cv). **Wait until I say go:** I have an uncommitted restyle to commit or drop first.
**What it is:** VIT students work out GPA/CGPA (grades S–F on a 10-point scale, weighted by credits), with a profile, a share card with a QR code, and an admin page. It's plain HTML, `css/style.css`, `js/`, a small Turso API and a service worker. Run with `node tools/serve.js`; tests are `node tools/test-calc.js` and `node tools/test-server.js`.
**The ask:** make it the student tool people screenshot and share:
- One clear visual direction on every page (dark by default).
- **Mobile first,** with a **grade picker** instead of typing (none exists yet) and big tap targets.
- An animated result plus a grade breakdown chart.
- A share card worth posting.
- Unchanged calculations (tests must pass), a CI workflow for the two test files, and accessibility.
> Prompt: *"Read every HTML page, css/style.css and js/ in vitcgpacalc. Propose 2–3 distinct visual directions as static mock pages (index + calc at 375 px and 1280 px) and stop for the owner to pick one. Then apply it to every page, mobile first, with a grade picker, an animated result and a better share card. Keep all calculation logic and tests passing; screenshot every page at both widths."*
Skills that suit this: `impeccable`, `frontend-design`, `design-taste-frontend`.

### Priority 3: good to have (~7%)

| Id | Size | Repo | Task |
|---|---|---|---|
| P3-1 | L | notes-tools | Shared helper scripts (the repo is just a README). Spec: `new-subjects/SCRIPTS-TO-BUILD.md`, `sync-shell.js` first. |
| P3-2 | M | roblox-gfx-pipeline | **Film checker** (ffmpeg): repeated or frozen frames, shot lengths, subtitle timing, loudness vs `docs/SUPERIOR_PLAN.md`. Test on a generated clip. |
| P3-3 | S–M | roblox-gfx-pipeline | **gate.py false fails:** the light default `[80,130]` (`gate.py:28`) fails night scenes; blockers count the face-rig plate (`gate.py:243-256`). Fix these and add the first `tests/`. |
| P3-4 | S–M | roblox-gfx-pipeline | **Reusable animated subtitle module** (only a one-off Pillow caption exists). |
| P3-5 | M | roblox-gfx-pipeline | **Signature mouth in lip sync** (e.g. zigzag teeth): `face_rig.py`/`lipsync.py` have no handling yet. Code on a test rig; I check on real avatars. |
| P3-6 | M | drive-reel-mcp | Editor: time chip, crop and per-shot speed exist. Still missing: **speed ramps inside a shot** and **picking the song section on the waveform**. |
| P3-7 | M | getnotespro | Store growth: C1 sitemap, robots.txt, JSON-LD and OG tags (also on the 5 sites, S12); C3 free sample; C7 bundle (the entitlement flag exists, no product or checkout); C9 semester pass; C2/C5/C6/C8. |
| P3-8 | M each | notes sites | EVS features still missing: N1 exam countdown, N4 offline install (no service worker yet), N7 battle leaderboard, N6 score share card. |
| P3-9 | S | bachy101-notes | Run `tools/mcq-check.py` on all 269 EVS MCQs (only 152 checked) and list suspects in a PR. |
| P3-10 | L | baeng101-notes | S09: move the English site onto the shared shell (it still has its own `modules.js`, `settings()` and mode switches). |
| P3-11 | — | notes-roadmap | Token-saving scripts T1–T6 (`token-savings/`). |

**On hold:** the video pipelines (`minecraft-video-editing`, `roblox-video-editing`) are generated from another repo of mine, so PRs there would be overwritten.

---

## Part D: budget and how to spend it

| Tier | Share of your ~35% weekly |
|---|---|
| Priority 1 | ~6% |
| 2A security | ~6% |
| 2B parity | ~8% |
| 2C admin settings | ~3% |
| 2D CGPA redesign | ~5% |
| Priority 3 | ~7% |

If a task runs well past its size, stop, open a draft PR with what's done, and move on.

**How to save usage:**
- **Strongest model** for design and tricky logic (CGPA directions, security, admin settings). **Sonnet** for mechanical work (porting, tests from a spec, CI).
- **Build once on EVS, port with a script.**
- **Fresh session per task;** point Claude at the spec and the exact files.
- **Plan first for L tasks.**
- **Quick checks while working;** the full browser run only before the PR.

---

## Not yours (needs me)
Deploying; the Cloudflare, Razorpay, Turso, Vercel and Google dashboards (rate-limit rules, old-link redirects, the UPI display name, backup hosts, the $5 plan decision); confirming the `_mode.json` settings (P1-3) and running the S06 migration; real sign-in and payment checks; course content (thin tabs, missing figures, past papers, English FAT answers, physics papers); VIT's portal; my avatars, footage and Blender scenes.

## Talking to me
Open a PR and message me the link. If a spec is unclear or seems wrong, ask in the PR. I'm in India (IST, UTC+5:30).
