Status: PLAN (nothing built). Parent: `../SUBJECT-TYPES.md`.

# Playbook: mathematics (BAMAT205 Discrete Mathematics and Linear Algebra)

About 161-170 uploads on VTOP (rows and syllabus not read). Theory only: no lab pages.

## Notation rendering
| Option | Cost | Verdict |
|---|---|---|
| Today's `.eq` + `sub`/`sup` + `.frac` | 0 | Fine for one-line formulas; cannot do matrices, quantifiers with limits, aligned derivations |
| KaTeX in the browser | about 270 KB script plus CSS and fonts on every visit | No |
| KaTeX at build time (`katex.renderToString` in `tools/build.js`) | no client script; CSS about 25 KB and a few self-hosted font files; one dev dependency | Recommended |
| Hand-made SVG per formula | 0.25% per figure | Only for diagrams (graphs, Hasse diagrams, Venn), via the figure generators |

Authors write `<span class="tex">\forall x\,(P(x)\to Q(x))</span>` or `<div class="tex">…</div>`; the build
replaces it with KaTeX HTML. `validate.js` checks the source form (and that it parses), never the generated
spans. Recall mode keeps using `<b>` outside formulas. Confirm sizes in prototype P3 before adopting.

## Content format changes
- Tabs: The idea, Definition and notation, Worked example, Proof or derivation, Common mistakes.
- Proofs: `<ol class="proof">` with one step per `li` and a `<small>` reason; steps reveal one at a time
  (reuse the recall-style hide and show; small change in `hydrate()`).
- Questions: `numerical` and `explain` exist; add `type-in` (B2) with a normaliser for sets (`{1,2,3}` in any
  order), matrices (rows separated by `;`) and truth values.

## Checkers and small solvers (widgets on `viz-core.js`, step player where steps matter)
| Tool | Steps shown | Size |
|---|---|---|
| Truth table and equivalence checker | table | shared with systems playbook |
| Set operations and Venn (2-3 sets) | result | small |
| Relation properties (reflexive, symmetric, transitive, equivalence classes, closure by Warshall) | matrix per pass | medium |
| Graph: degree sequence, adjacency matrix, BFS/DFS, shortest path, Euler or Hamilton check | step player | shared with DSA widgets |
| Matrix: row reduction to echelon form with fractions, rank, determinant, inverse, 2x2 and 3x3 eigenvalues | each row operation | medium; exact fraction arithmetic, not floats |
| Linear system solver (unique, none, infinite) | row operations | on top of row reduction |

Checkers verify a student's answer and show the working; they are not a general computer algebra system.
Limit sizes (matrices to 4x4, sets to 8 elements) so output stays readable on a phone.

## App code to extend
`tools/build.js` (tex pass), `tools/validate.js` (tex source, `ol.proof`, `type-in`), `app/js/app.js`
`hydrate()` (proof step reveal), `qCard()`/`answerHTML()` (typed answers), new `viz-paid/` files,
new `tools/check-math.js` (each worked example's final answer recomputed by the solver functions).

## Unchanged
Flashcards, quiz MCQs, mock tests, papers, battle, formula sheet (`formulas[].html` may contain `.tex`).

## Cost
B5 4% once; `type-in` is part of B2. Subject: Lite 50-60% (proof-heavy drafting is the bulk) + 5 tools about
7% + figures by generators at 0.25% each.

## Quality risks
A wrong step inside a proof (no script catches logic: strongest model writes, a second pass re-derives, sample
by hand); row-reduction paths that differ from the faculty's but reach the same answer (say so); floats in
eigenvalues (keep to exact cases); TeX that renders differently in dark mode (use `currentColor`).

## Free and Pro
Free: notes, truth-table tool, questions without answers. Pro: step-by-step solvers, proofs revealed fully,
answers, formula sheet.

## Build order
1. P3 (one tab with build-time KaTeX, measure bytes). 2. B5. 3. Row-reduction tool and its checker script.
4. BAMAT205 Lite. 5. Relations and graph tools. The same playbook then covers BAMAT101.
