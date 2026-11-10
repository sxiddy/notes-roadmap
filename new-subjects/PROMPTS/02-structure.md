# Prompt 02: Structure (one session per module; mid-tier model is enough)

```
Subject [CODE] in C:\Users\Siddharth D\Projects\[folder]. Read only: BRIEF.md and src/clean/[module]/*.txt.
Do not read other files.

Produce tmp/outline-[module].json:
{ "module": "[m1]", "title": "...", "lectures": [ { "n": 1, "file": "<lecture file>", "title": "...",
  "topics": [ { "id": "[m1]-<kebab-name>", "title": "...", "tabs": ["...", "..."], "pages": [3,4,5],
                "figures": [ { "page": 4, "what": "labelled diagram of ..." } ] } ] } ] }

Rules: every slide heading must land in a topic or a tab. 3-6 tabs per topic, each tab one idea. Only real
diagrams, charts, maps and tables are figures, never stock photos. Ids are lowercase with hyphens and start with
the module id. Do not write any notes text. Do not invent topics that are not in the text.

Then run: node ../notes-tools/outline-check.js . [module]
Fix until it passes. Add the module to content/modules.json with its "lectures" list (n, title, topic ids).
Commit. Write one line in STATE.md with the usage figure.
```

Verification: `outline-check.js` (script). The owner glances at the topic list once per module.
