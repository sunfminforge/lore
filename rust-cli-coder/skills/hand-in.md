# Handing the work in

## Green gate

The repo's `gate.sh` is the definition of "may land": `cargo fmt --check`,
`./verify.sh` (Flux, `0 trusted; 0 ignored`), `clippy -D warnings`,
`cargo nextest run`. Run it whole, as it is, before you hand in; a part run
by hand is not the gate. When it fails, fix the code; when it fails for
something outside your scope (a toolchain missing on the machine, a test
another change broke), ask the foreman rather than working around it.

## README current

The README says what the tool is, how to install it, and sends the reader
to `--help`. When a command, an install line or a requirement changes,
the README changes in the same pull request. It never duplicates help;
it points to it.

## Before you hand in

- [ ] No pure crate does I/O; Flux says `0 trusted; 0 ignored`
- [ ] One JSON object per command; failure is `{"error"}` and non-zero
- [ ] An agent that never saw the tool runs its first command from
      `--help` and that command's `--help` alone
- [ ] `agent_help::check` passes in a test; help names nothing the reader
      cannot open
- [ ] Every error carries the line that fixes it
- [ ] Killing it mid-run and running it again loses nothing
- [ ] The gate is green and the README current
- [ ] Nothing changed outside the paths the task scopes

## Releases

You do not release, tag or publish unless the task says so. When it does,
the release is the repo's own script run at the tag's commit, the tag equal
to `Cargo.toml`'s version, and you do only what the task names. Otherwise a
change that is ready to release says so in its pull request and stops.

## The pull request

Open it in the repo the task changes, with `Closes <task>` in its body,
what changed, and how each acceptance item is met. Mark each item with
`guild task check <task> <item> --evidence "<what shows it>"`, the evidence
a command and what it printed. Then stop: the warden reviews and lands; you
never approve or merge, and you never push to the base branch.
