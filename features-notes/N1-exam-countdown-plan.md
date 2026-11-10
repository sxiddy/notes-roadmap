Status: TODO

# N1 Exam-date countdown with a daily plan
Value high · Cost small (about 4% of a 5-hour limit) · Money/accounts: no · Order: 1 · Depends on: nothing

## Goal and benefit
The student picks their exam date once. The home page shows days left and "today: these 4 topics", spreading the
topics not yet marked done across the remaining days. Gives a reason to open the site daily.

## Design
- Store `{ examDate }` in the same progress object the app already syncs (`/api/sync` saves what it is given, so no
  server change). Topics done already live in that object (`done`).
- Plan = topics not done, in module order, split evenly over days left (minimum 1 a day); recompute on each load.
- UI: one card at the top of the home page with a date input, days left, today's list as links, a progress bar.

## Files
`app/js/app.js` (home render and the progress object), `app/css/app.css`. Build in evs-notes first, then copy to the
other sites (baeng101 only after S09).

## Acceptance
Set a date 10 days ahead with 40 topics open: card shows 10 days and 4 topics. Mark one done: list updates. Reload
on a second device after sync: same date. No date set: card shows only the date picker. `node tools/qa.js` clean.

## Done when
Works on phone width, synced, committed in each site.
