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
- Guild's hub is Forgejo, which `gh` does not reach: open the pull with `guild tool forge-pull open`, as the instance's own account, and find your task's pulls with `forge-pull find`. Write a pull's or issue's number only once the call that made it has returned it. {#open-the-pull-on-forgejo}
- When the hub refuses your push (pre-receive) or the repo has pull requests off, leave the commits at the worktree's head for the warden, say so on the task, and ask the foreman how the work lands, early, before more is built on it; never route around the refusal. {#push-refused-leave-the-head}
- A restarted task looks for its pull before redoing anything: one open, approved but unmergeable after a sibling landed is rebased onto main, its gate rerun, the conflict commented; a task another landing already met still closes by merge, with an empty commit and each check's output as its evidence. {#restart-find-the-pull-first}
- Commit the `Cargo.lock` entries your change adds, as a `--locked` build refuses without them; on a rebase take main's lock and let cargo add yours, never `cargo generate-lockfile`, which moves other crates' pins. {#cargo-lock-on-rebase}
- A send-back about the reviewer's host, not the work (no cargo there, a variable missing in its instance), changes no code: recheck where the checks run, record the host and versions in the pull request, and say so. {#send-back-on-the-reviewers-host}
- An acceptance check whose command the current CLI does not meet (a default it expects that is opt-in) is met by changing the CLI, code and help together, named in the pull request; never by bending the check. {#acceptance-over-current-cli}
- A test outside your paths that your change makes untrue (a list of what is not yet built) is the foreman's to let you change: ask early for that one line; the sibling task that empties such a list removes the test. To see every test a change moves, run them with `--no-fail-fast`. {#out-of-scope-test-your-change-falsifies}

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
