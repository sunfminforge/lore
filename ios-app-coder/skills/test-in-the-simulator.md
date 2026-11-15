# Testing in the simulator

Reach for this on every change, and before anything else when something is
reported broken. Its red, green, refactor cycle and its diagnosis loop
follow Matt Pocock's tdd and diagnosing-bugs skills (MIT), whose commit
the adopting pull request pins.

## Where tests go

- Tests are Swift Testing (`@Test`, `#expect`, `#require`) on the logic module's public types, run by `xcodebuild test` on the simulator the gate names. A test that reaches a private part, or breaks on a refactor that kept behaviour, tests past the interface: move it to the public seam. {#test-at-the-seam}
- The in-memory store, a temporary directory, and the clock and seed passed in are the only fakes; the logic module runs as it is, never mocked. {#only-fakes}
- An expected value comes from outside the code: a literal worked by hand, the task's example. A test that recomputes the answer the code's way agrees by construction. Parameterized tests (`@Test(arguments:)`) cover the cases; a property that must hold for every input says so in one test over many. {#expected-from-outside}
- A stored type has a round-trip test, and a changed one a test that opens a store written by the version before: skills/shape-an-app.md. {#test-the-store}
- A UI test (XCUITest) is for a flow no unit test reaches: it launches the app with a launch argument that opens an in-memory store with known data, and finds elements by accessibility identifier, never by position or text that a translation changes. {#ui-test-when-needed}

## Red, green, refactor

- One behaviour per cycle, a tracer bullet: one failing test at one seam, the least code that passes it, then the next. Tests written all ahead test a shape you imagined. {#one-behaviour-per-cycle}
- Red is seen, not assumed: run it and read it fail on its own `#expect`, not on a build error or a missing simulator. {#red-is-seen}
- Run one test while you cycle (`-only-testing:<target>/<suite>/<test>`) and the whole gate before you hand in. When a run fails, read the failing test's message from its result bundle (`xcrun xcresulttool`), not from the end of the log. {#run-one-then-all}
- Refactor only on green, with the tests as they are. {#refactor-on-green}

## When something is broken: the diagnosis loop

- Loop before theory: first one test, already run, that goes red on the symptom as reported, the same each run, unattended. Reading source for a cause before that test exists is the failure the loop prevents. {#loop-before-theory}
- A failure that comes and goes: pin the clock, the seed and the order of async work, run it repeated (`-test-iterations`, `-run-tests-until-failure`) until it goes red often enough to chase. With no loop after trying, ask the foreman with `guild task question`, naming what you tried. {#raise-the-rate}
- Minimise the input and the stored data one cut at a time, rerunning after each, until every part left is needed for red. Rank three to five causes, each with what it predicts, and test them one change at a time; a probe logs under one tag. {#cut-and-rank}
- The minimised case stays as the regression test, red before the fix and green after. Done when the first loop is green, a search for the probe's tag finds nothing, and the pull request names the cause. {#repro-becomes-the-test}

## The simulator

- The gate names its destination (`platform=iOS Simulator,name=…,OS=…`); a run by hand uses the same, so it checks what the gate checks. A destination missing on the machine is a question to the foreman, not a reason to change the gate. {#gate-names-destination}
- Simulator builds need no signing identity: when one is asked for, the project or the gate is wrong, and the fix is `CODE_SIGNING_ALLOWED=NO` for simulator builds in the gate where its task scopes it, never a certificate. {#no-signing-for-simulator}
- The simulators are the machine's, shared with others: boot, use and erase only the device your run made or names; delete none. {#shared-simulators}
