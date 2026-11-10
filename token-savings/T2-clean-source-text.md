Status: TODO

# T2 Script-clean the source text before any model reads it
Estimated saving: about 15% of drafting input (estimate)

## Waste today
Extracted slide text (`src/text/<module>/*.txt`, pypdf) carries page headers, footers, slide numbers, repeated
titles, "Thank you" slides and duplicate slides. The strongest model reads all of it.

## Change
Build `tools/clean-text.py` (spec in `new-subjects/SCRIPTS-TO-BUILD.md`):
- drop lines that repeat on more than 40% of pages (headers, footers, course code, lecturer name);
- drop page numbers and empty pages; merge hyphenated line breaks;
- collapse a slide that repeats the previous one with one extra bullet (build-up slides) into the last version;
- keep page markers `=== p12 ===` so figures and facts can be traced;
- write `src/clean/<module>/<lecture>.txt` and print before/after word counts.
Never rewrite or summarise: the script only deletes exact repeats.

## Measure
Word count before and after per lecture (the script prints it). Expect 10-25% fewer words on slide decks.

## Quality guard
Run the coverage script against the ORIGINAL text, not the cleaned one, so a wrongly deleted line shows up as a
missing fact. Image-only tables are still missing from any text extraction (lesson 9): the contact-sheet skim in
the pipeline stays.

## Done when
Script exists in the shared tools, tried on one EVS module with coverage unchanged.
