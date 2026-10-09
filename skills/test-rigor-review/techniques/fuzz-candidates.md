# Finding fuzz test candidates

## What to flag

Code that receives bytes or strings from outside the trust boundary:

- parsers, decoders, and deserializers
- file-format and protocol readers
- template, query, and path builders that take user text
- anything with manual index arithmetic, length fields, or unsafe memory access

Rank a target higher when its input arrives from the network or from uploaded files,
and when it is hand-written instead of generated.

## What each target asserts

State the assertion in the finding. From weakest to strongest:

1. Returns a value or an error; never crashes, hangs, or exhausts memory.
2. If an inverse exists, a value that parses successfully survives a round-trip.
3. If a second implementation exists, both agree on accept or reject.

Propose the strongest one the code allows. "Does not crash" alone is acceptable only
when no inverse and no second implementation exist.

## Property test or fuzz test

Use a property test when the input has structure the generator can build (an order, a
user, a list) and the rule is about business behavior. Use a fuzz test when the input
is raw bytes or text and the risk is in how it is read. A parser often deserves both:
a property test over generated valid documents, and a fuzz test over arbitrary bytes.

## Note in the finding

- Where the seed corpus comes from: existing fixtures and test inputs.
- That the run needs a time limit, and whether the project's CI already has a place
  for long-running jobs.
- Whether the code can be built for fuzzing at all. If it reads from a file or socket
  directly, report that as a blocker under refactors.
