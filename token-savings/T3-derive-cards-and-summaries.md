Status: TODO

# T3 Derive flashcards, one-liners and summaries from the finished notes with a cheaper model
Estimated saving: about 60% of that step (PLAN.md budgets about 5% of a limit per module for summary + questions + flashcards)

## Waste today
The strongest model writes flashcards and module summaries although they only restate notes it already wrote.

## Change
1. After a module's chunks pass validation, a mid-tier Claude model (Sonnet; PLAN.md already allows Sonnet for
   "flashcards from the finished notes"; never Haiku) gets ONE chunk file and returns flashcards per topic and the
   summary's big ideas, in the existing formats (`flashcards: [{front, back}]`, `SUMMARY({...})`).
2. `tools/derived-check.js` (spec in SCRIPTS-TO-BUILD.md) verifies: every number, formula and bold term on a card
   or summary line appears in that topic's notes text; no card back longer than 30 words; 4-8 cards per topic;
   no duplicate fronts.
3. Failures go back to the same model once with the failing lines; a second failure goes to the strongest model.
This is a PROPOSAL. Free models (FreeLLMAPI) stay check-only: they never write text students read.

## Measure
`get_usage` before and after the derive step for one module, against the same step done by the strongest model on
the previous module.

## Quality guard
The script check above, then the strongest model reads 10 random cards per module. If more than 1 in 10 is wrong
or misleading, drop this saving for that subject.

## Done when
One module done this way, both numbers logged, decision recorded here (keep or drop).
