# test-rigor-review

An agent skill that reviews existing code for where property-based tests, fuzz tests,
and mutation testing would catch real bugs, which existing tests of those kinds are
too weak to catch anything, and which refactors would make the code testable this way.

It produces a ranked findings report first, and writes tests or refactors only after
you pick findings.

## Install

```bash
npx skills add sunfmin/test-rigor-review -g
```

## Use

```
/test-rigor-review internal/billing
```

or ask in your own words: "where should we add property tests in this package?",
"what should we be fuzzing?", "are these tests actually checking anything?".

## Layout

- `skills/test-rigor-review/SKILL.md` — process, report format, standing rules
- `skills/test-rigor-review/techniques/` — one short guide per stage, loaded on demand

Tool recommendations in `techniques/tools.md` were verified on 2026-10-09.

## Acknowledgement

Several techniques follow lessons from
[hegeldev/hegel-skill](https://github.com/hegeldev/hegel-skill), restated here.
