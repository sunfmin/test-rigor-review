# Finding property test candidates

## Patterns

Flag a function when one of these applies, and name the pattern in the finding.

| Pattern | Signal in the code | Rule |
|---|---|---|
| Round-trip | encode/decode, marshal/unmarshal, parse/format, to/from pairs | decoding an encoded value returns the original |
| Invariant | totals, balances, counters, quotas, sorted or unique collections | the stated rule holds after every operation |
| Idempotence | normalize, dedupe, upsert, retryable handlers, migrations | applying it twice equals applying it once |
| Oracle | an optimized path beside a simple one; a rewrite beside the old code | both versions agree |
| Metamorphic | search, filter, pricing, ranking | a known change to the input causes a known change to the output |
| State machine | status fields with transition rules, workflows, caches | a random sequence of operations never reaches an illegal state and matches a simple model |
| Algebraic | merge, combine, set and money arithmetic | commutative, associative, or has an identity element |
| Rejection | validators, parsers, permission checks, constructors with preconditions | invalid input is refused, with no partial effect |

## Both directions

For every contract, look for two rules, and report them as separate candidates:

- **Accept** — what valid input must produce.
- **Reject** — what invalid input must not do: be accepted, crash, hang, or leave
  state half-changed. Validators and parsers hide most of their bugs here. The useful
  inputs are near-valid ones: a field one past its range, trailing garbage, a
  misplaced separator, a duplicate key.

## Sibling entry points

Entry points come in families: the methods of one interface, the same operation on
each type that implements it, several functions that take the same input shape. List
each member on its own line. A rule proven for one says nothing about the others, and
an input one member rejects is a candidate rule for every sibling that takes the same
shape.

## Evidence for a reject rule

A statement of what a function accepts implies what it refuses. "Parses a line
produced by Encode" is evidence that a line Encode could not produce should be
refused. Report such a rule and label its evidence as implied, so the user can
overrule it.

## Do not flag

- Glue code, trivial accessors, and UI layout.
- A function whose only available rule would restate its implementation.
- A rule with no evidence behind it, stated or implied. List it under "Not covered"
  with what would settle it.
