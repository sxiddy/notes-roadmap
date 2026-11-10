Status: TODO

# N8 Per-topic doubts answered by the owner
Value medium · Cost large (about 12%) · Money/accounts: accounts · Order: 8 (last; do only if students ask)

> **OWNER APPROVAL REQUIRED: touches accounts and creates moderation work for the owner.**

## Goal and benefit
A "Doubts" tab on each topic: students ask, the owner answers once, everyone studying that topic sees it.

## Design
- Reuse what exists: `api/feedback.js` already stores Help requests with the topic and the student email, the
  admin page already replies (`/api/stats` action `reply`), and replies reach the student's inbox.
- Add: admin action `publish { id }` copying question + reply (no email) into hash `doubts:<topicId>`; `GET
  /api/feedback?topic=<id>` returns published pairs. Nothing is public until the owner publishes it, so there is
  no open posting and no moderation queue beyond the existing inbox.
- Limit 30 published per topic; text is escaped on render (the admin page's `esc` exists; the app needs the same).

## Files
`api/feedback.js`, `api/stats.js`, `app/admin.html`, `app/js/app.js`.

## Acceptance
Ask a doubt as a student, reply and publish as owner, see it on the topic as a second student with no email shown.
A `<script>` in the text shows as text. Unpublished doubts never appear in the GET.

## Done when
Tested on one site with fakeredis, owner agreed to the workload, committed.
