# How to use this repo (every session reads this first)

This repo is a queue of self-contained plans for the VIT notes sites and the Notix store. Each plan file can be
executed by a fresh session with no other context. Written 11-11-2026 from a read-only audit.

## Rules (not optional)
1. Read `Projects/NOTES-SITES-LESSONS.md` first. Append every new mistake to it, with the fix.
2. Check usage (`get_usage`) before every step. Stop before 95% of the 5-hour limit (the figure is account-wide;
   other sessions move it). Write the item's status and what is left before stopping.
3. One agent at a time. No parallel sub-agents. Never Haiku for anything students read.
4. Never push or deploy until the owner says "push". Commit locally, one commit per item.
5. Never print, copy or commit a secret, key, token, password or env value. If you see one, say only that it exists.
   Env vars are named in these plans; their values are never written anywhere.
6. Files marked "OWNER APPROVAL REQUIRED" touch payments, referrals, admin or access. Show the owner the diff and
   wait for a yes before committing.
7. When an item is done: set its status line to `DONE <dd-mm-yyyy>`, update the README table, commit here.
8. Do not run `node tools/qa.js` in a site while another session is using its port (4183).
9. `git add` only the files you changed. In bachy105-notes, `tools/build.js` rewrites 6 stale generated files:
   do not commit them by accident.

## Repo layout (all under `C:\Users\Siddharth D\Projects`)
| Folder | What | Branch | Live |
|---|---|---|---|
| `bachy105-notes` | Applied Chemistry | main | bachy105.vercel.app |
| `baeee101-notes` | Basic Engineering | main | baeee101.vercel.app |
| `baeng101-notes` | Technical English (older shell, unfinished content) | main | baeng101.vercel.app |
| `evs-notes` | Environmental Sciences, code BACHY101 | master | bachy101.vercel.app |
| `vit-notes-store` | Notix store (Astro + Vercel functions) | redesign | notix-notes.vercel.app |
| `notes-roadmap` | this repo | | |

Each notes site has: `api/` (Vercel functions; files starting `_` are helpers), `app/` (static front end, the
Vercel output directory), `content/` (source notes: `chunk-*.js`, `q-*.js`, `summary-*.js`, `modules.json`),
`protected/` (built data and images, served only through `api/content.js` and `api/img.js`), `tools/`
(`build.js`, `validate.js`, `qa.js`, `fig.js`, `figs/`), `viz-paid/` (Pro interactive models).
GitHub: private repos under the `sxiddy` account (baeng101 is under a second account of the owner).

## How the pieces fit
- Sign-in: Google, student domain only (`STUDENT_DOMAIN`). Cookie `bn_user` on sites, `st_user` on the store.
- Pro is sold only on the store (Razorpay, UPI only). Each site asks the store `/api/entitlement` with
  `STORE_SECRET`. Store keeps `paid:<CODE>` hashes in Redis.
- One shared Upstash Redis. Sites prefix keys with `STORE_PREFIX` (`api/_store.js`); bachy105 and the store do not.
- Owner switches live in Redis hash `config` (per site), set on each site's `/admin`.

## Checks to run after any change
- Notes site: `node tools/validate.js --all` (0 errors), `node tools/build.js`, `node tools/qa.js`, then `git status`.
- Store: `npm test` (node test runner, `tests/*.test.js`, uses `tools/fakeredis.js` style fakes), `npm run build`.

## Which model does what
See `MODELS-AND-BUILD-METHODS.md`: the model and tool roster, how every feature is built, and who builds what
at each pipeline stage. Free models only check; they never write text students read.

## Resume
Open this repo, read this file, open `README.md`, pick the first TODO in the recommended order, read that one file.
