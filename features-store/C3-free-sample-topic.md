Status: TODO

# C3 A free, indexable sample topic per subject
Value high · Cost medium (about 6%) · Payment code: not touched (safe) · Order: 3

## Buyer benefit
The notes sites need sign-in, so a visitor (and Google) cannot see what the notes look like. One real topic per
subject on the store shows the quality and brings search traffic for topic names.

## Design
- New page `src/pages/[code]/sample.astro` (or a section in `[code].astro`) rendering one topic chosen by the
  owner per subject, as static HTML at build time.
- Source: a script `tools/sample.mjs` reads the topic from the subject repo's `content/chunk-*.js` (they call
  `CHUNK({...})`; evaluate with a stub `CHUNK` function, pick the topic id) and writes
  `src/data/samples/<CODE>.json` (tabs HTML, captions). Figures: copy that topic's SVGs from the subject's
  `figs-svg/` into `public/samples/<CODE>/`. Use only the site's own redrawn SVGs, never slide crops.
- Page ends with the price block and "Open the full notes" (existing components).
- Check `src/components/Demos.astro` first: if it already shows real note content, extend it instead.

## Files
`tools/sample.mjs`, `src/data/samples/`, `src/pages/...`, `public/samples/`, sitemap (C1).

## Owner input
Which topic per subject (pick one with a good figure and a high-search name).

## Acceptance
`npm run build` produces `/bachy105/sample/` with the topic text in the HTML source (not loaded by script), figures
visible, no Pro content (no flashcards, no answers). Listed in the sitemap.

## Done when
One sample per live subject, committed.
