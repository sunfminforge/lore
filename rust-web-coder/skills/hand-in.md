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

- [ ] No pure crate does I/O; one effect crate does all of it {#no-pure-crate-does-i-o-one}
- [ ] Only `GET`; the token comes from the environment, and a test shows it in no output, page, log or fixture {#only-get-the-token-comes-from}
- [ ] Fixtures are real answers, stripped; no test calls a live API {#fixtures-are-real-answers}
- [ ] Each bug fixed has a test that failed before the fix; no tagged probe remains {#each-bug-fixed-has-a-test-that}
- [ ] Each page renders without JS, in light and dark, at 360px {#each-page-renders-without-js-in}
- [ ] `check` and `serve` print one JSON object; a failure is `{"error"}` and non-zero, with the line that fixes it {#check-and-serve-print-one-json}
- [ ] The gate is green and the README current {#the-gate-is-green-and-the-readme}
- [ ] Nothing changed outside the paths the task scopes {#nothing-changed-outside-the}
- [ ] Each acceptance item's evidence is a command and what it printed {#each-acceptance-item-s-evidence}

## Never shipped by you

You do not release, tag, publish or deploy, nor start the service as
anything but a test or a local check. A change that is ready to ship says
so in its pull request and stops.
