# Shaping a CLI

The rules are skills/agent-cli.md; this is how to lay a tool out to keep
them.

## Pure and effect

- A crate is pure or it is not; no crate is mostly pure. When a change would put I/O in a pure crate, the effect crate reads it and passes what it read in as an argument. {#a-crate-is-pure-or-it-is-not-no}
- A bound is a type whose only constructor checks it (a key's length, a value's size, a version from 1); every later function takes the type, so nothing out of bounds gets past the edge. {#a-bound-is-a-type-whose-only}
- When the format is small (a file format, a number kind), own its reader in a pure crate rather than depend on a crate Flux cannot check, and say why in a decision record. Refuse past a fixed width (`too_large`) rather than add a big-number dependency or wrap. {#when-the-format-is-small-a-file}
- A reader the effect crate feeds a chunk at a time lets it choose: whole below a threshold, streamed above, never `read_to_end` on stdin. {#a-reader-the-effect-crate-feeds}
- Each other system's CLI gets one module per command, the only place that builds that command line. {#each-other-system-s-cli-gets-one}

## Refusals

- Every refusal is a variant of one enum in a pure crate: its code is the variant's name, its help the variant's doc comment, and it carries what its fix needs. {#every-refusal-is-a-variant-of}
- The answer names the fix as a key, not inside prose: `{"error": "<summary>", "faults": [{"code", "message", "fix"}]}`; `doctor` answers `findings`, each `{"finding", "fix"}`. A refused command changes nothing. {#the-answer-names-the-fix-as-a}
- Collect every fault before answering; stop early only where past a fault nothing more can be known (a broken byte stream). {#collect-every-fault-before}
- A command whose job is to fail a hook (a `check`) may exit non-zero on its finding: give it its own code, distinct from refusals, keep the refusal's shape, and name it in the top-level help. Exit codes are constants placed in help by placeholder. {#a-command-whose-job-is-to-fail-a}

## Writes

- A write is whole: to a temp file beside the target, flushed, renamed over it, so a kill leaves the old file or the new. {#a-write-is-whole-to-a-temp-file}
- Writes that may run at once take an exclusive lock on a file of its own (the rename replaces the target's inode); a read needs none. Without a lock, the help says to run one at a time. {#writes-that-may-run-at-once-take}
- Writing what is already there changes nothing, version included, and answers `"changed": false`; a write answers `changed` and takes `--dry-run`. A tool that only reads needs no dry run. {#writing-what-is-already-there}

## Tests

- Run the real binary; assert its JSON and its exit code. {#run-the-real-binary-assert-its}
- Stand in for each external CLI with a cargo example placed first on `PATH`; its state is one JSON file, it records calls and injects failures, and shares the real code's constants. {#stand-in-for-each-external-cli}
- A refusal has a test asserting its fix; a write one that runs it twice, one that kills it midway, and, where it locks, many processes at once. {#a-refusal-has-a-test-asserting}
- Where the tool can check something of its own repo (its README), the gate runs it there. {#where-the-tool-can-check}
