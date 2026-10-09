# Refactors that unblock these tests

Report these in their own table. Each finding names the blocker, the change, and the
test the change unlocks. A refactor with no test waiting behind it is out of scope.

| Blocker | Refactor |
|---|---|
| Rule logic mixed with database, network, or file calls | Extract a pure function that takes data and returns a decision; keep the I/O in a thin caller |
| Hidden inputs: current time, randomness, environment, globals | Pass them in as parameters |
| Parser reads from a file or socket directly | Split out a function that accepts bytes |
| Function takes a large object and uses a few fields | Narrow the parameters, so inputs are cheap to generate |
| Valid values can only be assembled by hand in many steps | Add a constructor that enforces validity; the generator calls it |
| State stored as strings or as combinations of booleans | Replace with an enumeration, so illegal states cannot be built |
| Invariant exists only in comments or in people's heads | Extract it as a predicate the test can call |
| Function mutates state and returns nothing | Return the result, or the list of effects, as a value |
| Encoder with no public decoder, or the reverse | Expose the inverse, enabling a round-trip rule |
| Output order depends on map iteration, concurrency, or timestamps | Make the order explicit, or inject the source of nondeterminism |

## Limits

- Propose the smallest change that unblocks the test. Do not bundle other cleanup.
- Prefer building the input inside the test when that is enough. Do not add
  production code that only tests would call; for example, when a documented layout
  lets the test assemble valid bytes, no public encoder is needed.
- Keep behavior identical. If existing tests cover the code, they must pass unchanged
  after the refactor; if none do, say so, because the refactor then has no safety net.
- When a blocker is costly to remove, say what weaker test is possible without it.
