# Testing the binary

Adapted from the tdd and diagnosing-bugs skills of Matt Pocock's skills
collection, at commit 5863806 (MIT); the pull request that adopted them
links the source. The repo's `gate.sh` and decision records outrank this
page; its words come from the repo's `GLOSSARY.md`.

## Where tests go

- A test crosses the interface a caller crosses: a pure crate's public functions, or the binary's arguments, stdout and exit code. A test that reaches a private part, or breaks on a refactor that kept behaviour, tests past the interface: move it to the interface. {#test-at-the-interface}
- Run the real binary; assert its JSON and its exit code. {#run-the-real-binary-assert-its}
- Stand in for each external CLI with a cargo example placed first on `PATH`; its state is one JSON file, it records calls and injects failures, and shares the real code's constants. {#stand-in-for-each-external-cli}
- Stand-ins sit only where the world begins: another system's CLI, a temp directory for files, the clock and seed passed in. A crate of your own is called, never mocked. {#stand-ins-only-where-the-world}
- An expected value is worked out apart from the code: a literal by hand, the spec's example, the other system's real output. Never compute it the way the code does; a `proptest` states a property (a round trip, an order, a bound), not the code again. {#expected-from-outside-the-code}
- A refusal has a test asserting its fix; a write one that runs it twice, one that kills it midway, and, where it locks, many processes at once. {#a-refusal-has-a-test-asserting}
- Where the tool can check something of its own repo (its README), the gate runs it there. {#where-the-tool-can-check}

## Red, green, refactor

- One behaviour per cycle: one failing test at one interface, then the least code that passes it, then the next. Never all the tests first and the code after: tests written ahead test a shape you imagined. {#one-behaviour-per-cycle}
- Red is seen, not assumed: run it, and read that it fails on its own assertion, not on a compile error or a missing stand-in. In a pure crate the red may be a refinement Flux refuses: skills/pass-flux.md. {#red-is-seen}
- Refactor only on green, with the tests left as they are; a test that must change for a refactor that kept behaviour was testing past the interface. The gate runs after. {#refactor-on-green}

## When something is broken

- No theory before a loop. First one command, already run, that goes red on the symptom as reported (the JSON key, the exit code, a stand-in's recorded call), the same each run, in seconds, with nobody at it: a failing test, the binary on a fixture input, a `proptest`, or `git bisect run` when it once worked. {#loop-before-theory}
- A failure that comes and goes: raise its rate (loop it, run it in parallel, pin the seed and the clock passed in) until it comes often enough to chase. With no loop after trying, ask the foreman with `guild task question`, naming what you tried; never guess a fix. {#raise-the-rate}
- Cut the input, the flags and the stand-in's state one at a time, running the loop after each cut, until every part left is needed to go red; for a property, `proptest`'s shrinking does it. {#cut-to-the-smallest-repro}
- Rank three to five causes, each with what it predicts ("if this, changing that makes it pass"), and test them in order, one change at a time. A probe writes to stderr under one tag, never to stdout, whose one JSON object it would break; remove every tagged line before handing in. {#rank-falsifiable-causes}
- The cut-down repro becomes the regression test, at the interface where the bug happens, seen red before the fix and green after; then the first loop runs again. Where no interface reaches it, say so in the pull request: a shape to fix, never a test to fake. The pull request names the cause found. {#repro-becomes-the-test}
