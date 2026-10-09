# Auditing existing property and fuzz tests

Check each existing test against this list. For each failed check, quote the
offending line and give the smallest change that fixes it.

1. **Inputs narrower than the contract.** Ranges trimmed, characters filtered, sizes
   capped, or a class of values excluded, so that the generator can no longer reach
   the cases the function is supposed to handle.
2. **A fixed input dressed as a property.** The test only ever sees one value or a
   short hand-picked list.
3. **One direction only.** A parser or validator tested only on valid input, or
   invalid input checked only for "does not crash" when refusal should be asserted.
4. **Weak assertion.** Checks only success, only a length, or only that a value is
   present, where the actual value could be checked cheaply.
5. **Rule copied from the implementation.** The assertion recomputes the answer the
   same way the code does.
6. **Several rules in one test.** Unrelated contracts share one test, so one failure
   hides the others.
7. **Tolerance added to pass.** Approximate comparison where the contract is exact,
   or an epsilon wider than anything the documentation supports.
8. **Configuration pinned.** Constructor and configuration options held at defaults
   instead of generated.
9. **One variant tested.** The rule covers several types or modes and the test
   exercises one.
10. **Hidden failures.** Commented-out assertions, skipped or ignored tests, tests
    that expect a crash, or a TODO standing in for a red test.
11. **Too few runs.** The run count was lowered below the framework default, or was
    left at the default for a rule whose input space is large, such as free text or
    sequences of operations.
12. **Fuzz target with no corpus or no time limit**, or one that was added and is
    never run.
