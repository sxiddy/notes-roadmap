# Next session: free-access leak on BAPHY105, then free quiz battles on every site (owner, 18-11-2026)

## 1. URGENT: BAPHY105 may give Modules 2–5 away free
- `baphy105-notes/api/_access.json` (built by tools/build.js) has `freeModules: ['m1'..'m5']`, and `freeFiles` lists every
  chunk-Na.js and q-Na.js. content/modules.json marks m2–m5 `free:false`. Find why the build treats them as free:
  compare with baeee101-notes, where only the free modules are listed.
- Fix, rebuild, deploy, then check signed out and signed in unpaid: M2–M5 are locked, and their chunk files come back
  401/402 from protected/data.
- Then check the same thing on baeee101, baeng101, bachy105 and bachy101.

## 2. Free quiz battle: let students choose (owner's rule)
Now (every site, api/battle.js `allow()` + app.js picker ~l.2420–2440): a free account gets ONE preview battle on ONE
topic, and only from a free module.
Owner wants a free student to pick **any one module** (paid modules included) and **any topics inside it** (one or
more, at most one module per battle). Keep it to one preview battle per account (`bfree:<email>`), unless the owner
says otherwise.
- api/battle.js: replace the `topics.length === 1 && all in freeModules` rule with "all topics in the same module".
- app.js picker: in preview mode show every module, allow several topics, and disable topics in other modules once
  one is picked. Update the info and help text.
- Fix the BAPHY105 wording left over from BAEEE101: "one preview battle on one Module 2 topic" (app.js ~l.2384, 2430,
  api/battle.js l.39) and "plus Modules 3 and 4" (~l.2568). It must name this site's real modules.
- Do the same change on baeee101, baeng101, bachy105 and bachy101 (evs-notes): each has the same `allow()`.
- Run tests and fakeredis checks, then build, validate and qa on each site, then deploy each.
