# Shaping a CLI

The rules are skills/agent-cli.md; this is how to lay a tool out to keep
them.

## Pure and effect

- A crate is pure or it is not; no crate is mostly pure. When a change would put I/O in a pure crate, the effect crate reads it and passes what it read in as an argument. {#a-crate-is-pure-or-it-is-not-no}
- A bound is a type whose only constructor checks it (a key's length, a value's size, a version from 1); every later function takes the type, so nothing out of bounds gets past the edge. {#a-bound-is-a-type-whose-only}
- When the format is small (a file format, a number kind), own its reader in a pure crate rather than depend on a crate Flux cannot check, and say why in a decision record. Refuse past a fixed width (`too_large`) rather than add a big-number dependency or wrap. {#when-the-format-is-small-a-file}
- A reader the effect crate feeds a chunk at a time lets it choose: whole below a threshold, streamed above, never `read_to_end` on stdin. {#a-reader-the-effect-crate-feeds}
- Each other system's CLI gets one module per command, the only place that builds that command line. That module is a real seam, filled twice, by the command and by its stand-in on `PATH`, so it needs no trait; a trait or generic with one filler is indirection, not a seam. {#each-other-system-s-cli-gets-one}

## Deep crates

- A pure crate earns its place by depth: much deciding behind few functions, where its interface is all a caller must know, refinements, call order and refusals included. Run the deletion test: were it gone, would every caller re-check what its refinements now carry? If not, it only passes through; fold it into its caller. {#deep-crate-deletion-test}
- When a crate's shape is in question, design it twice: sketch two unlike interfaces (the fewest functions; the effect crate's commonest call made one line) and keep the one whose refinements stay smallest while callers need nothing more. {#design-a-crate-twice}

## Refusals

- Every refusal is a variant of one enum in a pure crate: its code is the variant's name, its help the variant's doc comment, and it carries what its fix needs. {#every-refusal-is-a-variant-of}
- The answer names the fix as a key, not inside prose: `{"error": "<summary>", "faults": [{"code", "message", "fix"}]}`; `doctor` answers `findings`, each `{"finding", "fix"}`. A refused command changes nothing. {#the-answer-names-the-fix-as-a}
- Collect every fault before answering; stop early only where past a fault nothing more can be known (a broken byte stream). {#collect-every-fault-before}
- A command whose job is to fail a hook (a `check`) may exit non-zero on its finding: give it its own code, distinct from refusals, keep the refusal's shape, and name it in the top-level help. Exit codes are constants placed in help by placeholder. {#a-command-whose-job-is-to-fail-a}

## Writes

- A write is whole: to a temp file beside the target, flushed, renamed over it, so a kill leaves the old file or the new. {#a-write-is-whole-to-a-temp-file}
- Writes that may run at once take an exclusive lock on a file of its own (the rename replaces the target's inode); a read needs none. Without a lock, the help says to run one at a time. {#writes-that-may-run-at-once-take}
- Writing what is already there changes nothing, version included, and answers `"changed": false`; a write answers `changed` and takes `--dry-run`. A tool that only reads needs no dry run. {#writing-what-is-already-there}
