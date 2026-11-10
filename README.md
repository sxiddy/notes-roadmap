# Notes roadmap

Every improvement for the four VIT notes sites and the Notix store, and the pipeline for about 20 more subjects,
as self-contained plans. Written 11-11-2026 from a read-only audit. No secrets are in this repo.

**To resume:** open the repo, read `HOW-TO-USE.md`, pick the next TODO in the order below, read that one file.
Models and how every feature is built: `MODELS-AND-BUILD-METHODS.md`.

Money = touches payments, referrals, admin or access (owner approval required). Cost = rough percent of one
5-hour limit (estimates).

## Index
| Id | Title | Value | Cost | Risk | Money | Status |
|---|---|---|---|---|---|---|
| S01 | Sites default to paid and signed-in | High | 2% | Low | yes | TODO |
| S02 | No referral cash on discounted orders | High | 3% | Low | yes | TODO |
| S03 | Withdrawals rechecked after refunds, 7-day hold | High | 5% | Medium | yes | TODO |
| S04 | Admin key: no URL form, guess limit, per-site keys | Medium | 5% | Medium | yes | TODO |
| S05 | Device limit for sign-ins without a device id | Medium | 3% | Low | yes | TODO |
| S06 | Redis prefixes for the store and Chemistry | Medium | 6% | High (data migration) | yes | TODO |
| S07 | Limit and validate `/api/track` | Medium | 3% | Low | no | TODO |
| S08 | Refunds logged once | Medium | 3% | Low | yes | TODO |
| S09 | Port English to the shared shell | Medium | 10% | Medium | yes | TODO |
| S10 | Gift email checks, move Pro action | Low | 3% | Low | yes | TODO |
| S11 | Stale text and price fallbacks | Low | 1% | Low | no | TODO |
| S12 | OG tags, sitemap, robots, JSON-LD | Low | 3% | Low | no | TODO |
| S13 | Generic 5xx errors, session expiry | Low | 3% | Low | yes | TODO |
| S14 | Check the webhook signature on Vercel | Unverified | 1% | Low | yes | TODO |
| T1 | Fresh session per lecture, 30-line brief | High | 1% | Low | no | TODO |
| T2 | Script-clean source text | Medium | 3% | Low | no | TODO |
| T3 | Derive flashcards and summaries with a cheaper model | Medium | 2% | Medium (quality) | no | TODO |
| T4 | MCQ split and checks | Medium | 3% | Medium (quality) | no | TODO |
| T5 | Figures by generators only | High | 0% | Low | no | TODO |
| T6 | Stop-doing list | Medium | 0% | Low | no | TODO |
| P1 | new-subjects/SCRIPTS-TO-BUILD.md | High | 15-20% once | Low | no | TODO |
| P2 | new-subjects/PIPELINE.md (first subject through it) | High | 50-60% per Lite subject | Medium | no | TODO |
| R1 | content-review: English (finish content first) | High | 30%+ | Low | no | TODO |
| R2 | content-review: Chemistry, Basic Engineering, EVS | Medium | 10-20% each | Low | no | TODO |
| N1 | Exam countdown and daily plan | High | 4% | Low | no | TODO |
| N2 | Weak topics | High | 4% | Low | no | TODO |
| N3 | Spaced flashcards | High | 8% | Low | no | TODO |
| N4 | Offline install | Medium | 8% | Medium | no | TODO |
| N5 | Cram sheet | Medium | 3% | Low | no (uses Pro gate) | TODO |
| N6 | Share score card | Medium | 3% | Low | reads referral | TODO |
| N7 | Battle leaderboard | Medium | 6% | Low | accounts | TODO |
| N8 | Topic doubts | Medium | 12% | Medium | accounts | TODO |
| C1 | Store SEO (see S12) | High | 3% | Low | no | TODO |
| C2 | Social-proof counters | High | 4% | Low | no | TODO |
| C3 | Free sample topic pages | High | 6% | Low | no | TODO |
| C4 | Sale and exam countdown | Medium | 2% | Low | no | TODO |
| C5 | Refund promise badge | Medium | 1% | Low | no | TODO |
| C6 | Referral progress bar | Medium | 2% | Low | reads referral | TODO |
| C7 | All-subjects bundle | High | 8% | Medium | yes | TODO |
| C8 | Two-sided referral | Medium | 6% | Medium | yes | TODO |
| C9 | Semester pass | High later | 12% | High | yes | TODO |
| L1 | later/razorpay-auto-payout.md | Low now | n/a | High | yes | LATER |

## New-subject planning files (added 11-11-2026)
| File | What |
|---|---|
| [new-subjects/VTOP-INVENTORY.md](new-subjects/VTOP-INVENTORY.md) | What VTOP lists for the Winter 2026-27 courses (PARTIAL) |
| [new-subjects/SUBJECT-TYPES.md](new-subjects/SUBJECT-TYPES.md) | Subject types, shared building blocks B1-B6, ETH/ELA split, prototypes, owner decisions |
| [new-subjects/types/programming.md](new-subjects/types/programming.md) | Playbook: BACSE104, BACSE105 |
| [new-subjects/types/systems-hardware.md](new-subjects/types/systems-hardware.md) | Playbook: BACSE103, BACSE106 |
| [new-subjects/types/mathematics.md](new-subjects/types/mathematics.md) | Playbook: BAMAT205 |
| [new-subjects/types/language.md](new-subjects/types/language.md) | Playbook: BAESP101 (blocked on source) |
| [new-subjects/types/physics-lab.md](new-subjects/types/physics-lab.md) | Playbook: BAPHY105 |
| [new-subjects/SUBJECTS.md](new-subjects/SUBJECTS.md) | Subject list; rows 1-7 are these courses, status planned |

## Recommended order
1. S01, S02, S03 (money at risk), then S11 (trivial).
2. R1 English content, then S09.
3. P1 scripts, T1-T6 (rules), then P2 with the first new subject.
4. C1/S12, C2, C5, C4, C3 (safe store work that raises sales).
5. S04, S05, S07, S08, S13, S14, S10.
6. N1, N2, N5, N3, then C6, N6, C7.
7. S06 (plan a quiet hour), R2, N4, N7, C8, C9, N8.

## Open questions for the owner
See the end of each plan under "Owner input". The main ones: the 20 subjects and their source material
(`new-subjects/SUBJECTS.md`); which rule for S02; the hold period for S03; whether the store and Chemistry share
one database (S06); whether the Razorpay webhook is set and delivering (S08, S14); refund wording (C5); exam
dates (C4); sample topics (C3); bundle percent (C7).
