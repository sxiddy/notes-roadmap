Status: PLAN (nothing built). Parent: `../SUBJECT-TYPES.md`.

# Playbook: programming and algorithms (BACSE104, BACSE105)

Languages seen on VTOP: BACSE104 is C then C++ (pointers, storage classes, dynamic memory, structures, unions,
file handling; then templates, STL, exception handling). BACSE105 rows were not read; assume C or C++ until the
inventory says otherwise.

## Content format changes
- Tab HTML: add `pre` to the allow-list. `<pre class="code" data-lang="c">…</pre>`; the build escapes `<`, `&`.
  Optional `data-hl="3,7"` to highlight lines. Keep code under 30 lines per block so it fits a phone.
- Trace table: `<table class="tbl trace">` (columns: step, line, each variable, output). Plain HTML, no code.
- Suggested tabs per topic: The idea, Syntax, Worked program, Trace, Common mistakes, Complexity (DSA only).
- New question types in `q-*.js`:
  - `output`: `{ type:'output', code:'…', options:[…4], correct }` or open with `accept:['5 7','5,7']`.
  - `fill`: `{ type:'fill', code:'for(i=0;i<n;__1__)', blanks:[{ id:1, accept:['i++','++i','i+=1'] }] }`.
  - `bug`: `{ type:'bug', code:'…', bugLine:4, fix:'…', options?:[…] }` (tap the faulty line).
- Lab topic (B4): `{ kind:'lab', exp:3, aim, algorithm, code, sampleIO:[{in,out}], viva:[{q,a}] }`.
- Complexity cheat sheet: reuse `formulas[]` (`name`, `html`, `note`) so `viewFormulas()` shows it unchanged.

## App code to extend (read each function first)
- `tools/validate.js`: allow `pre`, the new types, `kind:'lab'`; check every `blanks` id appears in `code`.
- `tools/build.js`: move `accept`, `bugLine`, `fix`, lab `code` and `viva[].a` into the Pro files (`qa-*`,
  `pro-*`). This is the security-sensitive part: free bundles must not contain answers.
- `app/js/app.js`: `hydrate()` adds the copy button and tokenizer (keywords, strings, comments, numbers: about
  60 lines, no library); `qCard()`, `answerHTML()`, `viewQuiz()` render and mark the three types; `viewLab()`
  becomes the generic lab list; `viewBattle()` unchanged (built from flashcards).
- New `app/js/viz-step.js` (B3) and widgets in `viz-paid/`: array sort, linked list, stack/queue, BST insert
  and delete, graph BFS/DFS, recursion call stack. Each widget is a pure `frames(input)` function plus a renderer.
- New `tools/run-snippets.js`: extracts every `pre.code` marked `data-run`, compiles with gcc/g++ on the build
  PC and compares stdout with the stated output. This is the quality gate; no model decides an output.

## Unchanged
Sign-in, paywall, sync, highlights, search, summaries, flashcards, recall mode (`<b>` blanking), papers, mock tests.

## Runnable code in the browser
| Option | Languages | Size and cost | Safety and limits | Verdict |
|---|---|---|---|---|
| None: verified traces, outputs, copy button | all | 0 | nothing to abuse | Recommended |
| Pyodide (CPython on WASM) | Python only | several MB first load, cached after; no server | runs in the tab; infinite loops need a worker and a kill timer | Only for a Python course (BACSE101) |
| JS interpreter for C/C++ (JSCPP-style) | subset of C++ | small | incomplete language support gives wrong errors on valid course code | No: wrong results are worse than none |
| Clang or TCC compiled to WASM | C, C++ | tens of MB | heavy on phones, slow first run | No |
| Server judge (Judge0, Piston) | all | needs an always-on box or paid API; Vercel functions cannot host compilers | abuse, queueing, cost per run | No at this price point |

Pair "None" with a copy button so students paste into the compiler they already use in lab.

## How existing features adapt
Quiz: mixes MCQ with `output`, `fill`, `bug`. Flashcards: front may hold a one-line `code`; add syntax and
complexity cards. Battle: unchanged; exclude cards with multi-line code. Papers: code answers use `pre.code`.
Mock tests: unchanged.

## Cost (percent of one 5-hour limit, estimates)
Shell blocks B1 4%, B2 6%, B3 4%, B4 5% (once). Per subject: Lite 50-60% (strongest model drafts; scripts and
gcc verify) plus widgets 1-1.5% each (6 for DSA is about 8%) plus lab pages about 0.5% per experiment.

## Quality risks
Wrong output in a trace (gate: `run-snippets.js`); undefined-behaviour examples that differ by compiler (state
the compiler); code too wide for phones (40-column soft limit); answers leaking into free bundles; lab solutions
copied into records (see SUBJECT-TYPES decision 3).

## Free and Pro
Free: notes with code, first trace per topic, questions without answers, one sample experiment.
Pro: all traces, answers and explanations, step visualisers, lab solutions and viva answers, complexity sheet.

## Build order
1. P1 prototype (pointers topic). 2. B1, B2 in the reference shell. 3. BACSE104 Lite. 4. B3 + two widgets (P2).
5. BACSE105 Lite with widgets. 6. B4 lab pages for both.
