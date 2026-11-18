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
