# Helping Notix: a work plan for a volunteer with spare Claude usage

Thank you for giving part of your Claude usage to this. This file assumes you know nothing about me or the projects, so it explains everything you need. Read **Part A** once (about 10 minutes), set up with **Part B**, then work through **Part C** in order. Each task in Part C has a prompt you can paste straight into Claude Code.

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
1. **Branch + pull request for everything.** Never push to `main`, `master` or `cloudflare`. You can't deploy anyway: I review, merge, deploy and check live.
2. **You get no secrets, and you don't need any.** Everything below runs locally with fakes. Never ask for or commit keys.
3. **Code first, content second.** You're not a VIT student, so prefer code tasks. Leave rewriting course content to me unless a task says otherwise.
4. **One task = one PR.** A shared change done on all five sites can be one PR per site, or one per task if you script it.
5. **Evidence in every PR:** the check commands you ran and their output, screenshots for UI changes, and a "not tested" list (normally: real Google sign-in, real payment, live Cloudflare).
6. At the end of each task, add a line to `notes-roadmap/HELPER-LOG.md`: task id, PR link(s), state. Change that spec's `Status: TODO` to `Status: IN PR #n`.

### Run a notes site locally
```bash
git clone https://github.com/sxiddy/bachy101-notes.git && cd bachy101-notes
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
git clone https://github.com/sxiddy/getnotespro.git && cd getnotespro && npm install
npm test                         # existing node --test suite in tests/
# create .dev.vars with obvious fake values (no example file exists yet; task 1.3 adds one)
npm run build && npx wrangler dev   # Astro site + Worker (config in wrangler.jsonc, code in worker/)
```
If a required variable isn't documented, list it in your PR. Task 1.3 fixes that properly.

### Claude Code tips for this repo
- Start each task in a **fresh session** with the prompt given. Long sessions waste usage.
- Useful skills, if you have them: `superpowers-writing-plans`, `superpowers-executing-plans`, `superpowers-verification-before-completion`, `addy-test-driven-development`, `addy-security-and-hardening`, `addy-incremental-implementation`, `anthropics-webapp-testing`. None is required.

---

## Part C: the tasks

Ordered by value per unit of your usage. Rough sizes: **S** = under 2% of a weekly limit, **M** = 2–5%, **L** = 5–10%. 35% of a weekly limit should cover Waves 1–3 and part of Wave 4. Stop wherever your budget ends: every task stands on its own.

### Wave 1: foundations that make everything after it cheaper (do these first)

**1.1 Shared test harness + CI on all five notes sites (M).** Today checks only run when someone remembers to.
> Prompt: *"Read README, NOTES-SITES-LESSONS.md and tools/ in this repo. Add a GitHub Actions workflow that, on every PR, runs npm ci, node tools/build.js, validate.js, qa.js, and a syntax check (`node --check`) of every file in app/js and api. Then add a Playwright smoke test that starts tools/serve.js with DEV_LOGIN=1, signs in as a dev student, opens home, one topic, one quiz (answers a question), and the papers page at 375 px and 1280 px, and fails on any console error. Make it pass locally, then copy the same setup to the other four sites."*

**1.2 `notes-tools`: one shared copy of the helper scripts (L).** Full spec in `notes-roadmap/new-subjects/SCRIPTS-TO-BUILD.md`. It ends five drifting copies of every script, and `sync-shell.js` shows exactly where the sites differ, which makes every later "apply to all five" task cheap.
> Prompt: *"implement notes-roadmap/new-subjects/SCRIPTS-TO-BUILD.md exactly, in the sxiddy/notes-tools repo. Build order as listed. Each script gets a fixture and a node --test / unittest. sync-shell.js first: run it across the five sites and save its drift report as notes-roadmap/content-review/SHELL-DRIFT.md."*
> The empty private repo `sxiddy/notes-tools` is ready for you.

**1.3 Store: required-env check + tests for payment logging (M).** Payments once silently stopped being recorded because one site was missing environment variables.
> Prompt: *"In getnotespro and in bachy101-notes, find every process.env / env.X read. Write ENV.md listing each variable, which site needs it, and whether it's secret. Add a startup check that returns a clear 500 'missing config: X' (without values) when a required variable is missing, plus .dev.vars.example with fake values. Add tests for the Razorpay webhook path that verify a payment is logged exactly once (see notes-roadmap/security/S08 and S14)."*

### Wave 2: security fixes (real money and accounts; all specced)
Each file in `notes-roadmap/security/` has the problem, the fix and how to test it. **Check first whether the code has already changed** (some specs are older than the move to Cloudflare). If it's already fixed, mark it and move on.

| Id | Size | One line |
|---|---|---|
| S01 paid-by-default | S | Content must be locked if the config fails to load, never unlocked |
| S14 webhook-raw-body | S | Verify the Razorpay signature on the raw body |
| S08 refund-logged-twice | S | A refund must be recorded once |
| S03 withdrawals-survive-refunds | M | Referral withdrawals vs later refunds |
| S02 referral credit on discounted orders | S | Credit based on the amount actually paid |
| S04 admin-key hardening | M | Admin auth: constant-time compare, rate limit, rotation |
| S05 device-limit bypass | M | The 1 phone + 1 laptop rule can be dodged |
| S06 redis prefix | S | Key collisions between sites sharing a database |
| S07 track endpoint | S | Visit counter abuse |
| S10 gift-email alias | S | Gmail-style aliases abusing gifts |
| S13 errors and sessions | M | Error leaks and session expiry |
| S09 port English to the shared shell | L | English site has drifted from the others; use the `sync-shell` report from 1.2 |
| S11 stale copy, S12 SEO meta | S | Cleanup |

> Prompt per item: *"Read notes-roadmap/security/SXX-*.md and NOTES-SITES-LESSONS.md. Confirm against the current code whether the issue still exists, with file:line evidence. If yes, write a failing test, fix it, make the test pass, and apply it to every affected site. PR description: issue, evidence, fix, test output."*

### Wave 3: features students will notice (specced in `features-notes/`, code-heavy)
Build each one on EVS first, then port it to the other four sites (this is where `sync-shell` pays off).

| Id | Size | Feature |
|---|---|---|
| N2 weak topics | M | Show each student their weakest topics from quiz history |
| N1 exam countdown plan | M | "X days left: do these topics today" |
| N5 cram sheet | M | One-page printable keyword sheet per module, generated from existing flashcards (no new writing) |
| N4 offline install | M | PWA install and offline reading |
| N7 battle leaderboard | M | Leaderboard for quiz battles |
| N6 share score card | S | Shareable image of a mock-test score |
| N3 spaced flashcards | S–M | Check against the new EVS "Review" tool first; it may already cover this |
| N8 topic doubts | L | Students ask a question on a topic; I answer from the admin portal |

Also from my recent work, port these EVS features to the other four sites (code is in `bachy101-notes` commit `072f6db`; Chemistry already has Fast track in `bachy105-notes` commit `002c73e`):
- **Review** (spaced repetition with "Knew it / Lucky guess"), **Mistake notebook**, **Mock test** (50 MCQs in 30 minutes), home card ("exam-ready" score + "Do this next")
- **Fast track page shell** for baeee101, baeng101 and baphy105. Wire up the page and the 10-MCQs-per-module mechanism. I'll write the per-subject fact lists myself.

**Quiz battle rule change (M):** spec in `notes-roadmap/new-subjects/TODO-BATTLE-AND-ACCESS.md`, section 2, for all five sites.

### Wave 4: the store, getting more students to buy (specced in `features-store/`)
| Id | Size | Feature |
|---|---|---|
| C1 SEO: sitemap and JSON-LD | S | So the sites show up in Google |
| C3 free sample topic | S | Try before you buy |
| C4 sale and exam countdown | S | Banner driven by settings |
| C7 all-subjects bundle | M | One payment for every course |
| C9 semester pass | M | |
| C2, C5, C6, C8 | S–M | Social proof, refund badge, referral progress, two-sided referral |

**Admin portal: everything editable (L).** I want to change any setting from the admin portal without touching code: prices (MRP/sale), discounts, which subjects are listed, banners, report replies and site links. Keep it tidy and easy to navigate.
> Prompt: *"In getnotespro, list every business value that's hard-coded (prices, subject list, discounts, copy, links) with file:line. Move them into a settings table in Turso with defaults, and add an admin 'Settings' page grouped by section with validation and an audit log of changes. The store and the five notes sites must read prices from it. Test with wrangler dev."*

### Wave 5: if budget is left
- **Token-savings scripts** (`notes-roadmap/token-savings/T1–T6`): mostly scripts that make future content work cheaper.
- **Performance:** measure requests per page load on a notes site (the free Cloudflare plan allows 100,000 a day) and cut them: cache headers, bundling chunks, fewer `/api/session` calls. Report before/after counts.
- **Accessibility pass** on the shared shell: keyboard use, contrast, labels.
- **Roblox GFX tooling** (`roblox-gfx-pipeline`, Python + Blender headless): build the "film checker" described in `docs/SUPERIOR_PLAN.md` (repeated or frozen frames, shot lengths, subtitle timing, loudness, via ffmpeg). It needs no assets from me.

---

## Part D: my other projects

### D1. VIT CGPA calculator: big UI redesign (L) — `sxiddy/vitcgpacalc`
**What it is:** https://vitcgpacalc.vercel.app is a free tool where VIT students work out their GPA and CGPA (VIT's grades are S, A, B, C, D, E, F on a 10-point scale, weighted by course credits). It also has a profile, a shareable result card with a QR code, and an admin page.
**Tech:** plain HTML pages (`index.html`, `calc.html`, `cgpa.html`, `profile.html`, `share.html`, ...), one stylesheet `css/style.css` (dark theme, CSS variables, Space Grotesk + Inter), scripts in `js/`, a small API in `api/` (Vercel functions, Turso), and a service worker `sw.js` for offline use. No framework.
**Run locally:** `node tools/serve.js`; tests: `node tools/test-calc.js` and `node tools/test-server.js`.
**The ask:** the UI works but looks generic. Make it look and feel drastically better, the kind of student tool people screenshot and share:
- A clear visual direction (type, colour, spacing, motion) applied to every page. Dark by default; add a light theme if it fits.
- **Mobile first.** Most students use it on phones. Entering grades and credits for 6–8 courses must be fast with thumbs: big tap targets, a grade picker instead of typing, no zooming.
- An instant, satisfying result: an animated GPA/CGPA number and a grade breakdown chart.
- A share card worth posting (`js/card.js`, `share.html`).
- Don't change what's calculated. `tools/test-calc.js` must still pass; add tests if you touch calculation code.
- Accessibility: contrast, keyboard use, labels.
> Prompt: *"Read every HTML page, css/style.css and js/ui.js in vitcgpacalc. Propose 2–3 distinct visual directions as static mock pages (index + calc at 375 px and 1280 px) and stop for the owner to pick one. Then apply the chosen direction to every page, mobile first, with a grade picker, an animated result and a better share card. Keep all calculation logic and tests passing; screenshot every page at both widths for the PR."*
Skills that suit this, if you have them: `impeccable`, `frontend-design`, `design-taste-frontend`.
**Note:** I have an unfinished restyle there that isn't committed. I'll commit or drop it before you start, so start from the latest `main`.

### D2. Roblox GFX/animation tooling (M) — `sxiddy/roblox-gfx-pipeline`
Python tools that drive Blender headless to make Roblox-avatar posters and short animated films. The plan and status table are in `docs/SUPERIOR_PLAN.md`; the quality rules are in its tables. My avatar files and renders stay with me, so build only tools:
- **Film checker:** given a rendered video, report repeated and frozen frames, shot lengths (cut detection), subtitle timing and loudness, using ffmpeg. Compare against the targets in `SUPERIOR_PLAN.md` (24 fps, median shot about 1.9 s, ...). Test on a short generated clip.
- **Zigzag talking mouth:** the face rig should keep an avatar's signature mouth shape (such as zigzag teeth) when the mouth opens for lip sync.
- More automatic checks in `tools/gate.py` for the rules in `SUPERIOR_PLAN.md`.

### D3. Video-editing pipelines (M) — `sxiddy/minecraft-video-editing`, `sxiddy/roblox-video-editing`
Scripts and rules for editing gaming videos (trailers, thumbnails) from raw clips with ffmpeg. Start from `MASTER_PLAN.md` in each repo.
- Make each pipeline run end to end on a tiny generated sample clip, with one command and a test.
- **Subtitles look weak:** a better default style (outline, shadow, size checked at phone width, optional word-by-word highlight).
- Tests for the scripts.

### D4. Drive reel tool (S) — `sxiddy/drive-reel-mcp`
An MCP tool that cuts short reels from clips. Add tests and clean up; read `README.md`, `AGENTS.md` and `HANDOFF.md` first. I'll commit my local changes before you start.

---

## Part E: spending your usage wisely
This is a rare chance for me, so please spend it where it buys the most.

**Budget split (of the ~35% weekly):**
| Area | Share | Why |
|---|---|---|
| Wave 1 foundations (CI, notes-tools, env check) | ~6% | Makes every later task cheaper and safer |
| Wave 2 security | ~6% | Protects real money |
| Wave 3 student features + porting | ~9% | What students notice and pay for |
| D1 CGPA redesign | ~6% | High-traffic free tool; a design task needs the best model |
| Wave 4 store + admin settings | ~5% | More sales, less of my time on edits |
| D2–D4 | ~3% | Tools only |

If a task runs well past its share, stop, open a draft PR with what's done, and move on.

**How to save usage:**
- **Use the strongest model for design and tricky logic** (D1 directions, security fixes, the admin settings design). **Use a cheaper model, such as Sonnet, for mechanical work**: porting a feature to the other four sites, writing tests from a spec, CI files.
- **Build once on EVS, then port with a script or `sync-shell.js`,** not by having Claude rewrite the same change five times.
- **Fresh session per task,** started with the task's prompt. Don't keep one giant session going.
- **Point Claude at the spec file and the exact files** instead of letting it explore the whole repo. The specs already did that exploration.
- **Plan first for L tasks:** ask for a short plan, check it, then let it implement. A wrong direction is the biggest waste.
- **Don't re-run the full browser test after every small edit;** run the quick checks, and do the full run before the PR.

---

## Not yours (needs me)
Deploying; Cloudflare, Razorpay, Turso and Google dashboards; real sign-in and payment checks on the live sites; writing course content; anything that needs VIT's portal.

## Talking to me
Open a PR and message me the link. If a spec is unclear or seems wrong, write your question in the PR rather than guessing. I'll answer when I'm online; I'm in India (IST, UTC+5:30).
