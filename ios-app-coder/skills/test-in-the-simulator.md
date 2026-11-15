# Testing in the simulator

Reach for this on every change, and before anything else when something is
reported broken.

## Where tests go

- Tests are Swift Testing (`@Test`, `#expect`, `#require`) on the logic module's public types, run by `xcodebuild test` on the simulator the gate names. A test that reaches a private part, or breaks on a refactor that kept behaviour, tests past the interface: move it to the public seam. {#test-at-the-seam}
- The in-memory store, a temporary directory, and the clock and seed passed in are the only fakes; the logic module runs as it is, never mocked. {#only-fakes}
- A stored type has a round-trip test, and a changed one a test that opens a store written by the version before: skills/shape-an-app.md. {#test-the-store}
- A UI test (XCUITest) is for a flow no unit test reaches: it launches the app with a launch argument that opens an in-memory store with known data, and finds elements by accessibility identifier, never by position or text that a translation changes. {#ui-test-when-needed}

## Running them

- A test is seen red before the code that makes it pass, failing on its own `#expect`, not on a build error or a missing simulator; a bug starts from a test that goes red on it as reported. {#red-is-seen}
- Run one test while you work (`-only-testing:<target>/<suite>/<test>`) and the whole gate before you hand in. When a run fails, read the failing test's message from its result bundle (`xcrun xcresulttool`), not from the end of the log. {#run-one-then-all}
- A failure that comes and goes: pin the clock, the seed and the order of async work, and run it repeated (`-test-iterations`, `-run-tests-until-failure`) until it fails often enough to chase; with none after trying, ask the foreman with `guild task question`, naming what you tried. {#flaky-repeated}

## The simulator

- The gate names its destination (`platform=iOS Simulator,name=…,OS=…`); a run by hand uses the same, so it checks what the gate checks. A destination missing on the machine is a question to the foreman, not a reason to change the gate. {#gate-names-destination}
- Simulator builds need no signing identity: when one is asked for, the project or the gate is wrong, and the fix is `CODE_SIGNING_ALLOWED=NO` for simulator builds in the gate where its task scopes it, never a certificate. {#no-signing-for-simulator}
- The simulators are the machine's, shared with others: boot, use and erase only the device your run made or names; delete none. {#shared-simulators}
