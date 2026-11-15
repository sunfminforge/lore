# Handing the work in

## Green gate

The repo's `gate.sh` is the definition of "may land", and what it runs is
its own to say (its Flux part: skills/pass-flux.md). Run it whole before
you hand in; a part run by hand is not the gate. Its scripts are committed
executable (`100755`): a mode of `100644` fails the warden's run. A new
repo's gate runs the tests with `cargo nextest run`, refusing with the
`brew` line when nextest is missing; an existing gate is changed only when
the task asks. When it fails for something outside your scope (a toolchain
missing, a test another change broke), ask the foreman rather than work
around it.

## README current

The README says what the tool is, how to install it, and sends the reader
to `--help`; it never duplicates help. When a command, an install line or
a requirement changes, the README changes in the same pull request.

## Before you hand in

Every rule of skills/agent-cli.md holds, and:

- [ ] An agent that never saw the tool runs its first command from `--help` and that command's `--help` alone {#first-command-from-help}
- [ ] Every refusal has a test asserting its fix {#refusals-tested}
- [ ] Killing it mid-run and running it again loses nothing {#survives-kill}
- [ ] The gate, run whole, is green; the README current {#gate-green-readme-current}
- [ ] Nothing changed outside the paths the task scopes {#stayed-in-scope}

## Releases

You do not release, tag or publish unless the task says so. When it does,
follow the source's release section: the repo's own script run at the
tag's commit, the tag equal to `Cargo.toml`'s version, and only what the
task names. Otherwise a change ready to release says so in its pull
request and stops.

## The pull request

Open it as your task says. Its body names any departure from the
agent-cli rules and the decision that records it; each item's evidence is
a command and what it printed. Then stop: the warden reviews and lands.
