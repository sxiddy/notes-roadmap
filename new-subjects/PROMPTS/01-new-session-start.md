# Prompt 01: start of any session on a new subject (paste as is, fill the brackets)

```
You are continuing the [CODE] [Subject name] notes website in C:\Users\Siddharth D\Projects\[folder].
Read, in this order and nothing else: C:\Users\Siddharth D\Projects\notes-roadmap\HOW-TO-USE.md,
C:\Users\Siddharth D\Projects\NOTES-SITES-LESSONS.md, [folder]\STATE.md, [folder]\CHECKLIST.md.
Then do exactly the next unticked step in CHECKLIST.md, following
notes-roadmap\new-subjects\PIPELINE.md for that stage.

Rules: call get_usage before every step and stop before 95% of the 5-hour limit. One agent at a time. Never
push or deploy unless I say "push". Never print a secret or env value. Facts come only from the course material
in src/. Commit locally after each finished unit. Before stopping, write one line in STATE.md: what is done,
what is next, the usage figure. Append any new mistake to NOTES-SITES-LESSONS.md.

Tell me the usage figure now, then start.
```

Notes for the owner
- `STATE.md` is a short file in the subject folder (10 lines at most). It replaces the long PLAN.md "State" log.
- Use prompts 02-06 for the specific stages; each is a fresh session.
