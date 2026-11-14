# Testing the binary

Reach for this on every change, and before anything else when something is
reported broken. Its red, green, refactor cycle and its diagnosis loop
come from Matt Pocock's tdd and diagnosing-bugs skills (MIT); the pull
request that adopted them pins the commit.

## Where tests go

- A test crosses the seam a caller crosses: a pure crate's public functions, or the binary's arguments, stdout and exit code. One that reaches a private part, or breaks on a refactor that kept behaviour, tests past the interface: move it to the seam. {#test-at-the-seam}
- Run the real binary; assert its JSON and its exit code. {#run-the-real-binary-assert-its}
- Stand in for each external CLI with a cargo example placed first on `PATH`; its state is one JSON file, it records calls and injects failures, and shares the real code's constants. That stand-in, a temp directory, and the clock and seed passed in are the only fakes; a crate of your own runs as it is. {#stand-in-for-each-external-cli}
- An expected value comes from outside the code: a literal worked by hand, the spec's example, the other CLI's real output. A `proptest` states a property (a round trip, an order, a bound); one that recomputes the answer the code's way is tautological and passes by construction. {#expected-from-outside-the-code}
- A refusal has a test asserting its fix; a write one that runs it twice, one that kills it midway, and, where it locks, many processes at once. {#a-refusal-has-a-test-asserting}
- Where the tool can check something of its own repo (its README), the gate runs it there. {#where-the-tool-can-check}

## Red, green, refactor

- One behaviour per cycle, a tracer bullet: one failing test at one seam, the least code that passes it, then the next. Tests written all ahead test a shape you imagined. {#one-behaviour-per-cycle}
- Red is seen, not assumed: run it and read it fail on its own assertion, not on a compile error or a missing stand-in. In a pure crate the red may be a refinement Flux refuses: skills/pass-flux.md. {#red-is-seen}
- Refactor only on green, before you hand in, with the tests as they are; the gate runs after. {#refactor-on-green}

## When something is broken: the diagnosis loop

- Loop before theory: first one command, already run, that goes red on the symptom as reported (its JSON key, its exit code, a stand-in's recorded call), the same each run, in seconds, unattended: a failing test, the binary on a fixture input, a `proptest`, or `git bisect run` when it once worked. Reading source for a cause before that command exists is the failure the loop prevents. {#loop-before-theory}
- A failure that comes and goes: pin the clock and seed passed in, loop it, run it in parallel, until it goes red often enough to chase. With no loop after trying, ask the foreman with `guild task question`, naming what you tried. {#raise-the-rate}
- Minimise: cut the input, the flags and the stand-in's state one at a time, rerunning the loop after each cut, until every part left is needed for red; for a property, `proptest`'s shrinking does it. {#cut-to-the-smallest-repro}
- Rank three to five causes, each with what it predicts, and test them in order, one change at a time. A probe writes to stderr under one tag, never to stdout, whose one JSON object it would break. {#rank-falsifiable-causes}
- The minimised repro becomes the regression test at the seam where the bug happens, red before the fix and green after; then the first loop runs again. Where no seam reaches it, that is the finding: the pull request says so, never a test faked nearby. {#repro-becomes-the-test}
- Done when the first loop is green, the regression test is in, a search for the probe's tag finds nothing, and the pull request names the cause found. {#bug-hunt-done}
