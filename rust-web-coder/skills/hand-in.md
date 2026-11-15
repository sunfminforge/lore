# Handing the work in

## Green gate

The repo's `gate.sh` is the definition of "may land", and what it runs is
its own to say. Run it whole, as it is, before you hand in; a part run by
hand is not the gate. When it fails for something outside your scope (a
toolchain missing on the machine, a test another change broke), ask the
foreman rather than working around it.

## README current

The README says what the service is, what it reads, the environment it
needs (each variable by name), how to run `check` and `serve`, and sends
the reader to `--help`. When any of these changes, the README changes in
the same pull request.

## Before you hand in

- [ ] No pure crate does I/O; one effect crate does all of it {#no-io-in-pure-crates}
- [ ] Only `GET`; the token comes from the environment, and a test shows it in no output, page, log or fixture {#get-only-token-hidden}
- [ ] Fixtures are real answers, stripped; no test calls a live API {#fixtures-are-real-answers}
- [ ] Each page renders without JS, in light and dark, at 360px {#page-without-js}
- [ ] `check` and `serve` print one JSON object; a failure is `{"error"}` and non-zero, with the line that fixes it {#one-json-object}
- [ ] The gate is green and the README current {#gate-green-readme-current}
- [ ] Nothing changed outside the paths the task scopes {#within-scope}
- [ ] Each acceptance item's evidence is its command, run literally as written, and what it printed; a run that passed 0 tests is no pass (a shell loop quoting several test filters as one runs none) {#evidence-per-item}
- [ ] Work carried from an earlier task, or sent back: the last comments of the task, its goal and each task it replaces read before the code, since the fault may be the placement, not your work; and closed is not landed (a re-split closes a task without merging its pull request): earlier work not on `origin/main` is carried only where those comments or the foreman say so {#closed-is-not-landed}
- [ ] Carried work keeps one open pull request, naming your task: the gate rerun at its head and its `Closes` line edited to your task; only where its pull request is closed, the same commits pushed as your task's branch, one opened and the old closed as superseded {#one-open-pull-request}
- [ ] Sent back or held only for what another role must give (a read-only token, a credential, an approval): no code changed and your own token, which can write, never standing in; the gate rerun at the same head, your own items re-verified, the pull request saying what it waits on and whose it is, the foreman asked to hold the task or give the item to its owner; then stop {#waits-on-another-role}
- [ ] The pull request opened from a pushed branch, never by AGit (`refs/for/main`): pushing the branch leaves an AGit pull unchanged and landing cannot fetch its head; one found is reopened from a branch at the same sha, title and body copied, the old closed as superseded {#open-from-a-branch}

## Never shipped by you

You do not release, tag, publish or deploy, nor start the service as
anything but a test or a local check. A change that is ready to ship says
so in its pull request and stops.
