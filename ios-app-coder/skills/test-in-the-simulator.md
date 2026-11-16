# Testing in the simulator

Reach for this on every change, and before anything else when something is
reported broken.

## Where tests go

- Tests are Swift Testing (`@Test`, `#expect`, `#require`) on the logic module's public types, run by `xcodebuild test` on the simulator the gate names. A test that reaches a private part, or breaks on a refactor that kept behaviour, tests past the interface: move it to the public seam. {#test-at-the-seam}
- The in-memory store, a temporary directory, and the clock and seed passed in are the only fakes; the logic module runs as it is, never mocked. {#only-fakes}
- A stored type has a round-trip test, and a changed one a test that opens a store written by the version before: skills/shape-an-app.md. {#test-the-store}
- A UI test (XCUITest) is for a flow no unit test reaches: it launches the app with launch arguments that open an in-memory store with known data and set the colour scheme inside the app (`.preferredColorScheme`), since the simulator's own appearance setting can report dark and render light; it finds elements by accessibility identifier, never by position or text that a translation changes, and holds on every device size: a TabView is a tab bar on one device and a rail of plain buttons on another, so a tab is found among `app.buttons`, never through `app.tabBars`. An identifier set on a container covers what its `.overlay` holds, so a screen's identifier goes on the container before the overlay. {#ui-test-when-needed}

## Running them

- A test is seen red before the code that makes it pass, failing on its own `#expect`, not on a build error or a missing simulator; a bug starts from a test that goes red on it as reported. {#red-is-seen}
- Run one test while you work (`-only-testing:<target>/<suite>/<test>`) and the whole gate before you hand in. A failure reported on another device or host is reproduced here first, on the same device model (`xcrun simctl list devices available` often has it, else create it) with `-destination id=<it>`, before any code changes. When a run fails, read the failing test's message from its result bundle (`xcrun xcresulttool`), not from the end of the log. {#run-one-then-all}
- A failure that comes and goes: pin the clock, the seed and the order of async work, and run it repeated (`-test-iterations`, `-run-tests-until-failure`) until it fails often enough to chase; with none after trying, ask the foreman with `guild task question`, naming what you tried. {#flaky-repeated}

## The simulator

- The gate names or picks its destination; a run by hand uses the same device, by id, so it checks what the gate checks. Under Guild your `xcrun simctl` and `xcodebuild` share your instance's own device set, empty until you fill it and gone when you end: when `xcrun simctl list devices available` lists none, create the device the gate needs (`xcrun simctl create <name> <devicetype> <runtime>`, its ids from `xcrun simctl list devicetypes` and `runtimes`), which touches no one else's. A runtime missing, a device that will not boot, or a wrapper that hides a device is the host: say on the task what you ran and what it printed, ask the foreman and end your turn, with nothing retried, restarted or bypassed and the gate unchanged. {#gate-names-destination}
- Simulator builds need no signing identity: when one is asked for, the project or the gate is wrong, and the fix is `CODE_SIGNING_ALLOWED=NO` for simulator builds in the gate where its task scopes it, never a certificate. {#no-signing-for-simulator}
