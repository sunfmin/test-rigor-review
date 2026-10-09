# Mutation testing

Mutation testing is a check you run, not a test you write. It measures whether the
existing tests would notice if the code were wrong.

## Where to recommend it

- Tests that were written quickly or by an agent.
- Packages with high coverage whose assertions look thin: checking only that no error
  occurred, only a length, or only that a value is present.
- Packages holding rules whose failure is expensive.

## Scope

A run recompiles and retests once per mutant, so whole-repository runs are slow.
Recommend one package or the files in the current diff. In the finding, name the
scope and whether the project already has the tool configured.

## Triage survivors

This happens after the user picks the run, not during the review. Give every
surviving mutant exactly one verdict:

- **Missing assertion** — the tests run this line and do not check its effect. Add the
  assertion. This is the common case.
- **Equivalent mutant** — the change cannot alter observable behavior. Note why in one
  line.
- **Dead code** — nothing can reach or observe this line. Report it as a finding;
  removing it is the user's decision.

Report the survivor count before and after, per package. Do not raise the score by
excluding files or disabling mutation operators.
