# Handing the work in

## Green gate

The repo's `gate.sh` is the definition of "may land": `cargo fmt --check`,
`./verify.sh` (Flux, `0 trusted; 0 ignored`), `clippy -D warnings`, the
tests. Run it whole, as `./gate.sh`, before you hand in; a part run by hand
is not the gate. Its scripts are committed executable (`100755`): the
warden runs `./gate.sh`, and a mode of `100644` fails it. A new repo's
gate runs the tests with `cargo nextest run`, refusing with the `brew`
line when nextest is missing; an existing gate is changed only when the
task asks. When it fails, fix the code; when it fails for something
outside your scope (a toolchain missing, a test another change broke), ask
the foreman rather than work around it.

## README current

The README says what the tool is, how to install it, and sends the reader
to `--help`; it never duplicates help. When a command, an install line or
a requirement changes, the README changes in the same pull request.

## Before you hand in

Every rule of skills/agent-cli.md holds, and:

- [ ] An agent that never saw the tool runs its first command from
      `--help` and that command's `--help` alone
- [ ] Every refusal has a test asserting its fix
- [ ] Killing it mid-run and running it again loses nothing
- [ ] The gate, run as `./gate.sh`, is green; the README current
- [ ] Nothing changed outside the paths the task scopes

## Releases

You do not release, tag or publish unless the task says so. When it does,
follow the source's release section: the repo's own script run at the
tag's commit, the tag equal to `Cargo.toml`'s version, and only what the
task names. Otherwise a change ready to release says so in its pull
request and stops.

## The pull request

Open it in the repo the task changes, with `Closes <task>` in its body,
what changed, any departure from the agent-cli rules and its decision, and
how each acceptance item is met. Mark each item with
`guild task check <task> <item> --evidence "<what shows it>"`, the evidence
a command and what it printed. Then stop: the warden reviews and lands.
