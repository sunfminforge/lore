# Shaping a CLI the agent-cli way

## The workspace

One cargo workspace.

- **Pure crates**, as many as the domain has parts. No I/O: the clock, the
  environment and randomness arrive as arguments. `publish = false`. A
  crate is pure or it is not; no crate is mostly pure.
- **One effect crate**, the binary. It reads the world, asks the pure
  crates what to do, and does it. `main.rs` is one line calling
  `main(args) -> ExitCode`.
- Dependencies stay few: `clap` (derive, env), `agent-help`, `anyhow`,
  `serde`, `serde_json`. Add one only when the task cannot be done without
  it, and say why in the pull request.
- Another system is reached through its own CLI, one module per command,
  the only place that builds that command line. No SDKs, no tokens handled.

When a change would put I/O in a pure crate, move the reading into the
effect crate and pass what it read in as an argument.

## The output contract

- Every command prints exactly one JSON object on stdout. Warnings go to
  stderr.
- A failure is `{"error": "…"}` and a non-zero exit.
- An expected outcome is an answer with exit 0, not an error.
- An error names every fault at once, each with the line that fixes it: a
  command to run, a flag to pass, a file to write. "Invalid input" is never
  enough.
- Any exception to these is named in the top-level help.

## Nothing in the process

- No daemon state, no cache that is the truth, no file only this tool
  understands.
- Every write is idempotent: run twice, same world.
- Kill it anywhere and the next run finishes the job from what it reads.

## Every tool has

- `doctor`: reads everything, changes nothing, names the fix of each
  finding.
- A command that says what would happen and changes nothing.
- `--version` stamped by `build.rs`: `1.2.3` for a release, otherwise
  `1.2.3+<commits>.g<commit>[.dirty]`.

A new tool gets these in its first change; a change to an old tool that
lacks one says so in its pull request rather than adding it unasked.

## Tests

- Run the real binary; assert its JSON and its exit code.
- Stand in for each external CLI with a cargo example placed first on
  `PATH`. Its state is one JSON file; it records calls and injects
  failures.
- Stand-ins and real code share the same constants.
- A refusal has a test that asserts the line that fixes it.
- A write has a test that runs it twice, and one that kills it midway and
  runs it again.

## Words and decisions

New terms go in `GLOSSARY.md`, decisions in the repo's decision records,
module and interface comments first, before the code they describe.
