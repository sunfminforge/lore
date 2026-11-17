# Testing on the host

Reach for this on every change, and before anything else when something is
reported broken.

## Where tests go

- The logic module's tests run on the Mac itself, with no simulator or emulator: `swift test` on the package, JVM unit tests (`./gradlew test`, `jvmTest` or `commonTest` run on the JVM), `dart test` or `flutter test`; `commonTest` uses only multiplatform test libraries (`kotlin.test`). A test that needs a device is in the wrong module, or tests a view. {#host-tests}
- Tests are on the module's public types. A test that reaches a private part, or breaks on a refactor that kept behaviour, tests past the interface: move it to the public seam. {#test-at-the-seam}
- The in-memory store, a temporary directory, and the clock and seed passed in are the only fakes; the logic module runs as it is, never mocked. {#only-fakes}
- A stored type has a round-trip test, and a changed one a test that opens a store written by the version before. {#test-the-store}
- A screen's behaviour is tested on the host where the way allows: a Flutter widget test under `flutter test`, a Compose UI test in the `test` source set under Robolectric. A UI test on the simulator or emulator (XCUITest, an instrumented Compose test, a Flutter integration test) is for a flow no host test reaches. Either opens an in-memory store with known data and finds elements by accessibility identifier, test tag or key, never by position or by text a translation changes. {#ui-test-when-needed}

## Running them

- A test is seen red before the code that makes it pass, failing on its own assertion, not on a build error; a bug starts from a test that goes red on it as reported. {#red-is-seen}
- Run one test while you work and the whole gate before you hand in. When a run fails, read the failing test's own message, from the result bundle or the test report, not the end of the log. {#run-one-then-all}
- A failure that comes and goes: pin the clock, the seed and the order of async work, and run it repeated until it fails often enough to chase; with none after trying, ask the foreman with `guild task question`, naming what you tried. {#flaky-repeated}
