# Writing the tests the user picked

## Before writing

Read how the project already uses the tool. If it does not, read the tool's
documentation and its installed source. Check the name and signature of everything
you call.

Put new tests in the project's existing test files, following its conventions. Write
one rule per test, so each failure points at one contract.

## Generators

- **Default to the full domain the contract allows**, including the awkward values:
  empty input, zero, negative numbers, the exact bounds, very long strings, control
  characters, duplicates. Generate configuration and constructor options too.
- **Add a targeted generator beside it** when the interesting behavior needs
  structured input, such as documents that parse or orders that are payable. Build
  those values directly from smaller generators. Filtering random values wastes runs
  and skews the distribution.
- **Limit resource use where values are built**, by capping collection sizes or
  nesting depth in the generator's construction. Do not shrink the range of the
  values themselves.

## Run counts

Run each rule thousands of times, not the framework default. If the suite becomes
slow, raise the count on the rules with the largest input spaces and leave the rest.

## Prove the test can fail

After writing a rule, break the code under test on purpose, confirm the test fails,
and restore the code. A rule that cannot fail is worse than no test. Record in the
summary which break you used.

## The scale probe

Once per data structure, outside the generators, build one very large or deeply nested
value and run every operation on it: format, copy, compare, hash, serialize, iterate.
Recursion limits and accidental quadratic behavior only appear at sizes that
generated values never reach.

## Fuzz targets

Seed the corpus from existing fixtures and test inputs. Give every run a time limit.
Commit the corpus location the project's tool expects.

## When a run fails

Follow `triage.md`. When the failure is a real bug and is fixed, keep the shrunk
counterexample as a named example test or corpus entry, so the bug stays fixed.
