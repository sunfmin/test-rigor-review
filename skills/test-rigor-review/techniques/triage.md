# Triage of failures

Keep a ledger of every failing test you observe while working. Each entry ends in
exactly one of two verdicts:

- **Bug in the code** — reduced to a minimal input, traced to a cause, and reported to
  the user with a standalone reproduction that you have run and watched fail.
- **Bug in the test** — with a note saying what was wrong with the rule or generator
  and how you fixed it.

There is no third verdict. "A precision limit", "a degenerate input", and "the
documentation does not say" describe how severe a finding is. They do not make it
disappear.

## Deciding

Decide from evidence. Check the documentation's exact words, sibling implementations
of the same operation, callers that depend on the value, and the changelog. One path
disagreeing with all its siblings is a finding even where the documentation is
silent.

When the failing rule states an exact contract, such as equality, ordering, a
round-trip, or a stated bound, a difference of any size is a finding.

## Forbidden moves

- Deleting, skipping, or commenting out a failing test.
- Narrowing the generator to exclude the value that failed.
- Adding or widening a tolerance after watching the test fail.
- Changing production behavior without the user's decision.

Each of these is allowed only after the finding has been reported and the user has
chosen that outcome.

## Final report

Write it from a fresh run of the full suite. Every failure in that run appears in the
report, and every earlier failure that no longer appears has its ledger entry cited.
