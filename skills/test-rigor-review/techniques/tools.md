# Choosing tools

Use what the project already has. This table applies only when the project has no
tool for a technique. Maintenance status was checked on 2026-10-09; confirm the
current release and the exact commands in each tool's documentation before use.

| Language | Property | Fuzz | Mutation |
|---|---|---|---|
| Go | rapid (`pgregory.net/rapid`); second choice Hegel (`hegel.dev/go/hegel`) | `go test -fuzz` | gremlins; try avito-tech/go-mutesting on large modules |
| TypeScript | fast-check; second choice Hegel (`@hegeldev/hegel`) | Jazzer.js | Stryker |
| Python | Hypothesis | Atheris; HypoFuzz for pure-Python code | mutmut; cosmic-ray on Windows |
| Rust | proptest; second choice Hegel (`hegeltest`) | cargo-fuzz; bolero to run one harness as both property and fuzz test | cargo-mutants |
| Swift | swift-property-based | libFuzzer (`-sanitize=fuzzer`) | Muter |

## Notes

- **Hegel** (`hegel.dev`) is the second choice for Go, TypeScript, and Rust, after
  the stable library listed first. Its libraries share one engine derived from
  Hypothesis, so generation and shrinking behave the same across languages. Python
  uses Hypothesis itself, and Hegel has no Swift library. Propose Hegel when the
  project already uses it, when the user asks for it, or when one team wants the same
  library across several of these languages. Tell the user about these two limits
  when you do:
  - It is in beta and its maintainers announce that breaking changes may occur. Pin
    the version.
  - It loads a prebuilt native library, published for Linux amd64/arm64, macOS arm64,
    and Windows amd64/arm64. It does not run on other platforms, including Intel
    Macs.
- Never migrate existing property tests from one library to another as part of this
  review.
- **Jazzer.js** is in maintenance mode, with dependency updates only since April
  2026. Use it for parsers and decoders; cover everything else with fast-check.
- **gremlins** documents that it handles very large modules poorly. Scope runs to one
  package.
- **mutmut** needs `fork()` and has no native Windows support.
- **Muter**'s last tagged release is from 2023, and fixes continue on the main
  branch. Install from the main branch.
- **swift-property-based** requires Swift Testing and has a single maintainer.
  Exhaust (`nesevis/exhaust`) is a newer alternative that adds stateful testing and
  coverage-guided fuzzing.
- **Swift libFuzzer** is generally unavailable in the toolchain bundled with Xcode.
  Plan to run it on Linux or with a swift.org toolchain, and only on code that does
  not import UI frameworks.
- Do not add SwiftCheck, zimmski/go-mutesting, or dvyukov/go-fuzz to a project; all
  three have stopped receiving updates.
