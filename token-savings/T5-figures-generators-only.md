Status: TODO

# T5 Figures: generators only, one screenshot per seven
Saving already measured: about 0.25% of a limit per figure with generators against about 3% hand-written (124 EVS figures)

## Rule set (from `evs-notes/tools/figs/PLAYBOOK.md`; copy that file into every new subject)
1. Never hand-write an SVG. One generator file per batch of 10-14 figures (`tools/figs/batchN.js`) using
   `tools/figs/lib.js` helpers; tables through `tools/figs/table.js` specs.
2. Draw from caption and alt text. Open the source image only when it holds data the text lacks.
3. One preview per seven figures (`tools/figs/phone-preview.js`, absolute path, headless browser screenshot, read
   the PNG once). Never view a figure that was not changed.
4. Validate every generated SVG with an XML parser before previewing (lesson 34). Check `todo.js`'s pattern
   against the subject's chunk format first (lesson 35).
5. Fresh session per batch; stop at 85-90%.

## Further savings (proposals)
- Lite tier ships slide crops first (`tools/fig.js`, crop = [x, y, width, height], lesson 29) and redraws only
  after the subject sells (see `new-subjects/PIPELINE.md`). Saves the whole redraw cost (about 30% of a limit
  for 120 figures) on subjects that do not sell.
- A shared figure library: many subjects reuse axes, flow boxes, cycles, tables. Keep `tools/figs/lib.js` in one
  place (see SCRIPTS-TO-BUILD.md "shared tools") so helpers improve for every subject.

## Measure
Figures per session and % of limit per batch, logged in the subject's `figs-svg/README.md` progress note.

## Quality guard
Phone-width preview check, dark-mode check (lesson 36), `node tools/build.js`, `validate.js --all`, `qa.js`.

## Done when
PLAYBOOK.md is in the shared tools and referenced by the pipeline (this is mostly a rule, not a build).
