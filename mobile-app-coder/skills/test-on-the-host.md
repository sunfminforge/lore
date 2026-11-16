# Testing on the host

Reach for this on every change, and before anything else when something is
reported broken.

## Where tests go

- The logic module's tests run on the Mac itself, with no simulator or emulator: `swift test` on the package, JVM unit tests (`./gradlew test`, `jvmTest` or `commonTest` run on the JVM), `dart test` or `flutter test`. A test that needs a device is in the wrong module, or tests a view. {#host-tests}
- Tests are on the module's public types. A test that reaches a private part, or breaks on a refactor that kept behaviour, tests past the interface: move it to the public seam. {#test-at-the-seam}
- The in-memory store, a temporary directory, and the clock and seed passed in are the only fakes; the logic module runs as it is, never mocked. {#only-fakes}
- A platform framework the simulator cannot run (Vision and ML pose detection, the camera, motion) sits behind a thin adapter proved on the host, where Apple's frameworks run on macOS, from recorded or still samples; say early what those tests prove and what they cannot (a drawn figure is no person to a pose model), so the gap is the task's to close. {#hardware-on-the-host}
- A stored type has a round-trip test, and a changed one a test that opens a store written by the version before. {#test-the-store}
- A UI test (XCUITest, Compose UI test, Flutter integration test) is for a flow no host test reaches: it opens an in-memory store with known data and finds elements by accessibility identifier, test tag or key, never by position or by text a translation changes. Where `xcodebuild` cannot reach the devices of your own simulator set, keep the test, leave the walk to the tester's task and say so, rather than reaching for the machine's default set. {#ui-test-when-needed}

## Running them

- A test is seen red before the code that makes it pass, failing on its own assertion, not on a build error; a bug starts from a test that goes red on it as reported. {#red-is-seen}
- Run one test while you work and the whole gate before you hand in. Have the test task name each test it ran and print its summary on one line (Gradle's `testLogging` or a test listener), so a green run shows the new test ran. When a run fails, read the failing test's own message, from the result bundle or the test report, not the end of the log. {#run-one-then-all}
- A failure that comes and goes: pin the clock, the seed and the order of async work, and run it repeated until it fails often enough to chase; with none after trying, ask the foreman with `guild task question`, naming what you tried. {#flaky-repeated}
