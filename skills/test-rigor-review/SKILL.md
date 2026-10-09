---
name: test-rigor-review
description: >
  Load this FIRST, before reading any code, whenever the user asks where property-based
  tests would be worth adding, which places should get property tests or fuzz tests,
  what to fuzz, whether existing tests actually check anything, to run or interpret
  mutation testing, or how to refactor code so it can be tested with generated inputs.
  Also triggers on "property test review", "test rigor review", "属性测试", "模糊测试",
  "变异测试", "哪些地方该写属性测试", "哪些该做模糊测试", "测试到底有没有测到东西". It reviews existing
  code for candidates for those three techniques, audits existing property and fuzz
  tests for weakness, finds the refactors that unblock them, reports ranked findings,
  and writes only what the user then picks. Not for writing ordinary example tests or
  test-first development (tdd), and not for general design review (complexity-review).
---

# Test rigor review

Example tests check only the inputs someone thought of. This skill reviews an existing
codebase for the places where three stronger techniques pay off, and for the refactors
that unblock them.

- **Property test** — state a rule that holds for all inputs; a framework generates
  many inputs and shrinks any failure to a minimal counterexample.
- **Fuzz test** — feed coverage-guided random bytes to code that handles untrusted
  input; look for crashes, hangs, and broken round-trips.
- **Mutation testing** — break the code in small ways automatically and rerun the
  tests; a mutant that survives marks an assertion that is missing.

## Process

1. **Fix the scope.** Use the path, package, or diff the user named. If they named
   nothing, pick the packages that hold business rules and parsing, and say which ones
   you picked. Do not review a whole repository by default.
2. **Read what exists.** Detect the language, the test framework, and which of these
   tools are already set up. Reuse what the project has. Propose a new dependency only
   when it has none, chosen from `techniques/tools.md`. If the existing tests do not
   build or run, report that first, because it blocks step 8.
3. **Find candidates** for each technique, using `techniques/property-candidates.md`,
   `techniques/fuzz-candidates.md`, and `techniques/mutation.md`.
4. **Audit existing property and fuzz tests** with `techniques/audit-existing.md`. A
   weak test is worse than a missing one, because it reports safety that is not there.
5. **Find testability blockers** with `techniques/refactors.md`.
6. **Rank** within each table by (cost of a bug here) × (chance this technique finds
   one) ÷ effort. Money, permissions, state machines, and parsers of outside input
   rank first.
7. **Report** in the format below, then stop. The review is read-only: you may run
   existing tests and other commands that change nothing, but do not edit files,
   install tools, or start fuzz or mutation runs until the user picks findings.
8. **Write what the user picked**, following `techniques/writing-tests.md`. Handle
   every failure with `techniques/triage.md`.

## Report format

Open with **What exists**: language, test framework, which tools are set up, and
anything that blocks running tests.

Then these sections, each ranked with the most valuable finding first. Give every
finding an ID (`T1`, `W1`, `R1`, `M1`). When a section has no findings, say so in one
line.

- **Test candidates** (`T`) — location, technique, pattern, the rule in one sentence,
  the evidence for the rule, effort (S/M/L). For a fuzz target, the pattern column
  holds the assertion it makes. A candidate that needs a refactor first says
  "needs R1"; its effort excludes the refactor.
- **Weak existing tests** (`W`) — location, which check failed, the offending line,
  the smallest fix. One row per failed check. A fix listed here is not repeated as a
  test candidate.
- **Refactors** (`R`) — location, blocker, proposed change, the tests it unlocks,
  effort.
- **Mutation runs** (`M`) — package or files to run on, why the tests there are
  suspect, and whether the tool is set up. Survivor counts come later, in step 8.
- **Not covered** — rules left out for lack of evidence and what would settle each,
  techniques with no candidate, and defects you noticed that none of the three
  techniques would catch.

Locations are `file:line`, or a line range when the finding spans one. Mark every
counterexample as predicted or confirmed; it is confirmed only if you ran it. To
confirm one during the review, use a throwaway file outside the repository.

End with the three things to do first and the reason for each. Findings that depend on
each other, such as a refactor and the test it unlocks, count as one. A blocker from
**What exists** may be one of the three.

## Rules that hold throughout

- **Every rule needs evidence.** Take it from documentation, names and signatures,
  assertions already in the code, existing tests, or an ordinary expectation you can
  name (a round-trip, two equivalent paths agreeing). A rule that restates the
  implementation verifies nothing; leave it out.
- **A finding is a rule a person can check in one sentence.** If you cannot state it
  that briefly, it is not ready to report.
- **A failing new test is a finding.** Stop and report the counterexample. Never
  weaken the rule, narrow the inputs, or change production behavior to get green.
- **Learn each tool's API from the project** — existing tests first, then the tool's
  documentation, then its installed source. Do not write syntax from memory.

## Before stopping

Check the work against each technique file you used. Anything in those files that you
skipped gets a stated reason under **Not covered**.

- `techniques/property-candidates.md` — rule patterns, both directions, sibling entry points
- `techniques/fuzz-candidates.md` — what to fuzz and what each target asserts
- `techniques/mutation.md` — where to run it and how to triage survivors
- `techniques/audit-existing.md` — checks for property and fuzz tests already present
- `techniques/refactors.md` — blockers and the smallest change that removes each
- `techniques/writing-tests.md` — generators, run counts, scale probe, proving a test can fail
- `techniques/triage.md` — the failure ledger and its two verdicts
- `techniques/tools.md` — which tool to choose per language
