# Next session: free quiz battles on every site (owner, 18-11-2026)

All notes are free on every site (Pro is the extras listed on Notix). Item 1 below was a mistake and is withdrawn: BAPHY105 modules.json had m2-m5 free:false, fixed in d3fca5a.

## 2. Free quiz battle: let students choose (owner's rule)
Now (every site, api/battle.js `allow()` + app.js picker ~l.2420–2440): a free account gets ONE preview battle on ONE
topic, and only from a free module.
Owner wants a free student to pick **any one module** (all modules are free) and **any topics inside it** (one or
more, at most one module per battle). Keep it to one preview battle per account (`bfree:<email>`), unless the owner
says otherwise.
- api/battle.js: replace the `topics.length === 1 && all in freeModules` rule with "all topics in the same module".
- app.js picker: in preview mode show every module, allow several topics, and disable topics in other modules once
  one is picked. Update the info and help text.
- Fix the BAPHY105 wording left over from BAEEE101: "one preview battle on one Module 2 topic" (app.js ~l.2384, 2430,
  api/battle.js l.39) and "plus Modules 3 and 4" (~l.2568). It must name this site's real modules.
- Do the same change on baeee101, baeng101, bachy105 and bachy101 (evs-notes): each has the same `allow()`.
- Run tests and fakeredis checks, then build, validate and qa on each site, then deploy each.

## 0. FIRST after the 5-hour reset: full BAPHY105 audit (owner said "yes after reset", 18-11-2026)
Compare the live https://baphy105.vercel.app with the chem site (bachy105-notes) and with Notix's promises
(notix-notes.vercel.app/baphy105, vit-notes-store/src/data/subjects.mjs). Fix every real difference:
1. Text left over from BAEEE101 in app/js/app.js, api/*.js, the HTML and the guide text: "Module 2", "Modules 3 and 4",
   op-amp, circuits, CAD, lab bench, and wrong module counts.
2. Free versus Pro must match chem: all notes free, Pro is only the Notix extras. Diff modules.json, _access.json,
   _mode.json and the gate against bachy105-notes.
3. Every chem feature (mock CAT/FAT, revision pack, formula sheet, battles, bookmarks, timer, papers with notes links)
   works or is clearly absent. Notix must not promise anything missing.
4. Live browser check signed out: every page, the console and network, and 375 px. Say what still needs a signed-in test.
Run build, validate, qa and syllabus-check, then commit, push and deploy. Report each difference as fixed or not, with
evidence. Already fixed 18-11: env placeholders, OAuth origin, ₹13 price, m2-m5 marked paid.
