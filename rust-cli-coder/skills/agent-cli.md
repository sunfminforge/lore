# The agent-cli way

The source is sunfmin's agent-cli skill (MIT); the pull request that
adopted it pins the commit.

Where the repo carries a copy, read it whole before you start a new tool or
touch a release. This page is
the rules you keep from it; the skills it names say how. A repo's own copy
of the skill and its decision records outrank both, and a departure from a
rule here is named in the repo's decisions and in its top-level help, never
taken silently.

## The rules

- **Rust, one cargo workspace.** Few dependencies: `clap`, `agent-help`, `serde_json`, and `serde` or `anyhow` only where needed. Another system is reached through its own CLI, never an SDK, and no token is handled. {#one-workspace-few-deps}
- **Pure crates, checked by Flux.** As many as the domain has parts; no I/O, the clock, environment and randomness passed in; `publish = false`. Every function verifies at `0 trusted; 0 ignored`: skills/pass-flux.md. {#pure-crates-flux}
- **One effect crate**, the binary: it reads the world, asks the pure crates, and does what they decide. `main.rs` is one line: skills/shape-a-cli.md. {#one-effect-crate}
- **Help is the only manual**, written with agent-help on the type that is each fact's home, and kept whole by `agent_help::check`: skills/write-help.md. {#help-only-manual}
- **One JSON object on stdout** for every command, exit 0 for every answer, an expected outcome included. Warnings go to stderr. {#one-json-object}
- **Refusals carry the line that fixes them.** A failure is `{"error": …}` with a non-zero exit, naming every fault at once, each with the command, flag or edit that fixes it. {#refusal-carries-fix}
- **Nothing in the process.** Every write idempotent; killed anywhere, the next run finishes from what it reads. {#idempotent-writes}
- **Every tool has** `doctor`, a way to see what a write would do and change nothing, and a `--version` stamped by `build.rs`. {#doctor-dry-run}
- **One gate**, `gate.sh`: fmt, Flux, clippy `-D warnings`, the tests. It is the definition of "may land": skills/hand-in.md. {#one-gate}

## What the source leaves to the task

Release and packaging (Homebrew, Scoop, the five platforms) are the
source's, but you cut a release only when the task says so:
skills/hand-in.md. Words go in `GLOSSARY.md`, decisions in the repo's
decision records, comments before the code they describe.
