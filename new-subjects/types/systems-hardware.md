Status: PLAN (nothing built). Parent: `../SUBJECT-TYPES.md`.

# Playbook: systems and hardware (BACSE103, BACSE106)

BACSE103 syllabus is in `../VTOP-INVENTORY.md` (number systems, K-map, combinational and sequential circuits,
Booth multiplication, division, datapath, cache, IO, ARM). BACSE106 syllabus was not read; the tools below assume
the usual scheduling, synchronisation, deadlock, memory and file topics and must be checked against it.

## Content format changes
- No new tags beyond B1 (`pre.code` for assembly and shell snippets) and plain `table.tbl` for truth tables.
- Widgets are placed as today: `<div data-viz="id" data-preset="…"></div>`; presets carry the course examples.
- Suggested tabs: The idea, Worked example (table or timing diagram), Tool, Exam pattern.
- Questions: existing `numerical` and `interpret` cover most; add `type-in` (B2) for conversions and addresses.

## Shared simulator core (keeps each tool cheap)
All tools sit on B3 `app/js/viz-step.js`: `frames(input) -> [{state, note}]`, one player (prev, next, play), and
four shared renderers: `table`, `gantt`, `boxes` (registers, frames, memory cells), `wire` (gates and signals).
A new simulator is then a pure function of 30-80 lines plus a preset list, about 1-1.5% each instead of 3%.

| Tool | Course | Renderer | Notes |
|---|---|---|---|
| Base converter, 1s and 2s complement, IEEE 754 breakdown | 103 | boxes | free tool, good for store traffic |
| Truth table from an expression | 103 | table | small parser for AND OR NOT XOR |
| K-map (2-4 variables) with grouping steps | 103 | boxes | Quine-McCluskey for the groups; check against known cases |
| Gate and adder, mux, decoder, flip-flop, counter stepping | 103 | wire | fixed circuits only, not a free-form editor |
| Booth multiplication, restoring and non-restoring division | 103 | table | step per cycle |
| Cache mapping and page replacement (FIFO, LRU, optimal) | 103, 106 | boxes | one tool, two presets |
| CPU scheduling (FCFS, SJF, SRTF, priority, round robin) | 106 | gantt | waiting and turnaround table |
| Banker's algorithm, resource-allocation graph | 106 | table | safe sequence steps |
| Paging and segmentation address translation | 106 | boxes | |
| Disk scheduling | 106 | gantt (line) | if in syllabus |

## App code to extend
`app/js/viz-core.js` (keep), new `app/js/viz-step.js`, new `viz-paid/viz-m*.js` per module, `WIDGETS.md` ids,
`tools/validate.js` (accepts only registered ids, already true), `tools/viz-snap.js` for screenshots. Add
`tools/check-sims.js`: every preset's final answer must equal the worked answer in the notes.

## Unchanged
Everything else: tabs, recall, flashcards, quiz, mock tests, papers, battle, paywall.

## Cost
B3 4% once. BACSE103: Lite 50-60% + 6 tools about 8% + lab pages (C, Arduino, OpenMP) about 5%.
BACSE106: Lite 50-60% + 5 tools about 6% (renderers reused) + lab about 5%. Strongest model writes tools and
notes; scripts verify.

## Quality risks
A simulator that disagrees with the faculty's convention (tie-breaking in SJF, arrival at the same time, K-map
grouping choice): state the convention on the tool and match the slides. Free-form circuit editors are a
time sink: do not build one. Timing diagrams are hard to read on phones: scroll horizontally, 4 signals maximum.

## Free and Pro
Free: base converter and truth-table tool (traffic), notes. Pro: all step simulators, answers, lab solutions.

## Build order
1. P2 prototype (bubble sort + FCFS on one player). 2. Number tools and K-map. 3. BACSE103 Lite. 4. Scheduling,
page replacement, Banker. 5. BACSE106 Lite. 6. Remaining tools by student requests.
