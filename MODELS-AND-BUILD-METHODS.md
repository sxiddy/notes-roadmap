# Models, tools and how each feature is built

Written 11-11-2026 from the code and the build notes (`evs-notes/PLAN.md`, `tools/figs/PLAYBOOK.md`,
`NOTES-SITES-LESSONS.md`). "Today" = what the repos do now. PROPOSAL = not done yet. Costs are in percent of one
5-hour limit; only those marked (measured) were measured. App features marked (*) were confirmed present in
`app/js/app.js` by name only; read the code before changing them.

## 1. Model and tool roster
| Model / tool | Job | Stages | Must NEVER | Input | Output | Verified by | Cost per subject | Failures seen | Fallback |
|---|---|---|---|---|---|---|---|---|---|
| Strongest Claude (main session; Opus in PLAN.md, or the current top model) | Writes notes, MCQ stems and keys, model answers; reviews flags; writes figure generators and interactive models | Draft, Polish, Figures, Models, Questions | Invent facts; read big docs in drafting sessions; push | One lecture's cleaned text + BRIEF.md | `CHUNK`, `QUESTIONS`, `SUMMARY` files; generator JS | `validate.js`, coverage script, MCQ cross-check | Notes about 0.1% per slide page (measured: 66 pages = 7%); whole subject 1.3-1.7M tokens today | Context filled to 75% in one module; answer-letter drift (32 of 54 on B) | Smaller sessions (T1); `mcq-rotate.js` |
| Mid-tier Claude (Sonnet) | Today: mechanical work only (figure crops, flashcards from finished notes, validation fixes). PROPOSAL: outline, wrong options, explanations, summaries | Structure, Verify | Write notes or answer keys | One chunk or one module | Same file formats | `derived-check.js`, `outline-check.js`, strongest-model sample | About 5-8% (estimate) | Sub-agent stalled on one huge write; 140-230k tokens per chunk agent | Strongest model |
| Cheap Claude (Haiku) | Not used. Owner rule: never | none | Anything | | | | 0 | | |
| Fable | Not part of the build pipeline. This audit and roadmap were written in a Fable session | none | | | | | 0 | | |
| Free models through FreeLLMAPI (local router; GLM, Qwen, Gemini Flash, router `fusion`) | Check only: answer every MCQ from the notes; (tried) list facts as verbatim quotes | Verify | Write anything students read; decide numbers, years, names | Notes of one topic + questions without keys; one lecture | JSON answers; JSON quotes | `mcq-check.py` compares with the key; `freellm.py` keeps only quotes found word-for-word | Free; saves little | Single-model routes all 429; fact lists 413/429 and never saved; only `fusion` answered (152 of 269 MCQs, 0 disagreements) | Skip; strongest model samples 10 per module |
| pypdf (`tools/build.py`) | PDF text per lecture | Extract | | PDFs in `src/pdfs` | `src/text/<m>/*.txt` | Contact-sheet skim | 0 | Image-only tables missing (lesson 9); Unicode crash without `PYTHONIOENCODING` | Transcribe from page image |
| OCR | None today. PROPOSAL only if a subject is scanned | Extract | | | | Coverage against page images by hand | unknown | | Strongest model reads the page image |
| `tools/fig.js` | Crops slide figures (`crop: [x, y, w, h]`) | Figures (Lite) | | Page renders | `protected/img/*.webp` | Contact sheet look | about 1% | Stray page text in 9 of 25 crops | Inset 1.5% |
| `tools/figs/*` generators | Redrawn SVG figures | Figures (Full) | Hand-written SVG | Captions + alts | `figs-svg/*.svg` | XML parse, `phone-preview.js` screenshot | 0.25% per figure (measured) vs 3% by hand | Duplicate attribute; dark-mode contrast | Fix generator |
| `tools/validate.js` | Format, ids, banned phrases, thin tabs | every stage | | content files | errors, warnings | itself | 0 | | |
| `tools/build.js`, `admin-index.js` | Builds `app/`, `protected/`, `api/_access.json`, `_battle.json`, `_content.json` | Build | | `content/`, `viz-paid/` | generated files | `git status`, `qa.js` | 0 | bachy105 rewrites 6 stale files | Commit only your files |
| `tools/qa.js` (jsdom / headless browser), `viz-snap.js` | Opens every route, tab, diagram | Verify | Run while another session uses port 4183 | built app | error list, screenshots | itself | 0 | bachy105 cannot run it (missing package) | `validate.js` only |
| Human (owner) | Topic list glance, phone check, purchase test, all "push" | Structure, Launch | | | | | | | |

Who writes what: notes, MCQ stems/keys, model answers, generators, interactive models = strongest Claude.
Flashcards and summaries = strongest today; mid-tier PROPOSED. Wrong options = strongest today; mid-tier PROPOSED.
Free models and scripts only check.

## 2. Feature build map: notes sites
| Feature | Built from (data flow) | Content by | Script-only part | Cost per subject | Tested by |
|---|---|---|---|---|---|
| Notes tabs, figures | `content/chunk-*.js` -> build -> `protected/data/chunk-*.js` -> `/data/` -> `api/content.js` -> `app/js/app.js` | Strongest | build, crop | bulk of the subject | validate, coverage, qa |
| Module summaries | `content/summary-m*.js` | Strongest (mid-tier proposed) | | part of 5% per module | validate |
| Quiz (questions free, answers Pro) | `content/q-*.js` -> build splits into `q-*.js` and `qa-*.js` | Strongest | split, `mcq-rotate` (proposed) | part of 5% per module | validate, mcq-check |
| Flashcards, recall mode (*) (Pro) | flashcards in chunks -> `pro-<chunk>.js` (`PRO_CHUNK`) | Strongest (mid-tier proposed) | split | part of 5% per module | derived-check (proposed) |
| Battle | `api/_battle.json` (built from flashcards) + `api/battle.js`, Redis `bt:*` | none new | all | 0 | fakeredis run |
| Highlights (*), streak (*), timer (*), progress | browser storage + `api/sync.js` (`prog:<email>`) | none | all | 0 | qa |
| Formula sheet (*), lab bench (*), revision pack and PDF save (*) | in `app/js/app.js`; subject-specific parts must be rewritten per subject (lesson 5) | Strongest | | small, per subject | qa |
| Mock tests (*), papers | `papers-free.js`, `papers-pro*.js` (`PAPERS`); none for EVS | Strongest (transcribe + answers) | | only if papers exist | `check-papers.js` (baeee101) |
| Interactive models | `viz-paid/*.js` -> `protected/data/viz-*.js`; free ones in `app/js/viz/`; `app/js/viz-core.js` | Strongest | copy | Full tier only | `viz-snap.js`, qa |
| Search (*) | built in the app from loaded topics | none | all | 0 | qa |
| Sign-in, device limit | `api/login.js`, `session.js`, `_lib.js`, `app/js/gate.js` | none | all | 0 | manual + S05 tests |
| Paywall, Pro gating | `api/_access.json`, `content.js`, `img.js`, `_lib.js hasAccess` -> store `/api/entitlement` | none | all | 0 | anonymous 401 check |
| Report a mistake, free-tool label | `app/js/notix-extras.js` -> store `/api/community` | none | all | 0 | manual |
| Owner admin | `app/admin.html`, `api/stats.js`, `_notix.js` | none | all | 0 | manual |
| Planned N1-N8 | see `features-notes/` | none (N5 script-built) | all | 0 per subject once built | each file's acceptance test |

## 3. Feature build map: store (`vit-notes-store`)
| Feature | Files | Content by | Tested by |
|---|---|---|---|
| Pages (home, subject, legal, requests, updates) | `src/pages/*.astro`, `src/data/subjects.mjs`, `site.mjs`, `legal.json` | Strongest writes blurbs once per subject | `npm run build` |
| Cart, checkout, verify, webhook | `api/create-order.js`, `verify.js`, `_lib.js`, `src/scripts/core.js` | none | manual purchase; no dedicated test file seen |
| Discount codes | `api/_offers.js`, `offers.js` | none | `tests/offers.test.js` |
| Referrals, withdrawals | `api/_referrals.js`, `offers.js`, `public/js/admin-referrals.js` | none | `tests/referrals.test.js` |
| Gifts | `create-order.js`, `_lib.js` | none | `tests/gifts.test.js` |
| Reviews, requests, updates, mistake reports | `api/community.js`, `src/scripts/reviews.js`, `requests-page.js`, `updates*.js` | students / owner | `tests/community.test.js` |
| Entitlement for sites | `api/entitlement.js` | none | none seen (add one) |
| Admin, refunds, reconcile | `api/admin.js`, `public/admin.html`, `public/js/admin*.js` | none | `tests/offline.test.js` covers offline payments |
| Alerts, weekly backup | `api/cron.js`, `_notify.js` | none | manual |
| OG images | `tools/og.mjs` | script | look at the PNG |
| Planned C1-C9 | see `features-store/` | none | each file's tests |

## 4. Who builds what for a new subject
| Stage | Model / tool | Lite | Full | Verification |
|---|---|---|---|---|
| Extract | pypdf + `clean-text.py` (scripts) | yes | yes | contact-sheet skim |
| Structure | Mid-tier Claude (PROPOSAL; strongest today) | yes | yes | `outline-check.js`, owner glance |
| Draft | Strongest Claude | yes | yes | validate, coverage |
| Verify | Scripts + free models | yes | yes | scripts |
| Polish | Strongest Claude, flagged items only | yes | yes | validate, qa |
| Figures | `fig.js` crops / generators by strongest Claude | crops | redrawn | contact sheet / phone preview |
| Models | Strongest Claude | no | 3-5 | `viz-snap.js`, qa |
| Questions | Strongest (stems, keys) + mid-tier (options, PROPOSAL) | yes | yes | rotate, mcq-check, sample of 10 |
| Papers | Strongest Claude | free list only | worked answers | `check-papers.js` |
| Launch | Scripts + owner | yes | yes | `launch-check.js`, test purchase |
