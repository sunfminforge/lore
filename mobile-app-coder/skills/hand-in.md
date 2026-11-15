# Building, running and handing in

## The simulator and the emulator

- The gate names its iOS destination (`platform=iOS Simulator,name=…,OS=…`) and its Android emulator (an AVD name and API level); a run by hand uses the same, so it checks what the gate checks. A destination or system image missing on the machine is a question to the foreman, not a reason to change the gate. {#gate-names-devices}
- Builds for them need no signing identity: an iOS simulator build is unsigned (`CODE_SIGNING_ALLOWED=NO` where it asks for one), an Android build is the debug variant with the SDK's own debug key. A build that asks for a certificate or a release keystore is wrong, never fed one. {#no-signing}
- The simulators and emulators are the machine's, shared with others: boot, use and erase only the device your run made or names, run an emulator headless (`-no-window`), and stop what you started; delete none. {#shared-devices}
- Run means launched: the app installed on each, started, and its first screen up without a crash in the log (`xcrun simctl launch`, `adb shell am start`, then the log read). {#run-means-launched}

## Green gate

The repo's `gate.sh` is the definition of "may land": the host tests, then
the build and launch on both platforms. Run it whole, as it is, before you
hand in; a part run by hand is not the gate. Its scripts are committed
executable (`100755`). When it fails for something outside your scope, ask
the foreman rather than work around it.

## README current

The README says what the app is, what it keeps on the device and how
children's data is kept, the toolchains it needs, and how to run the host
tests, the gate and the app on each platform. When any of these changes,
the README changes in the same pull request.

## Before you hand in

- [ ] The logic module imports no UI framework, and every behaviour the task adds has a host test on it, seen red before green {#logic-tested-apart}
- [ ] A changed stored type has a migration and a test that opens the version before {#migration-tested}
- [ ] A search of the sources and manifests for network clients, the `INTERNET` permission, analytics, cloud sync and in-app purchase finds nothing the task did not ask for {#nothing-leaves-the-device}
- [ ] No name, icon, text, art or data of another app is in the repo; the app's text is in the task's language, in string resources {#nothing-copied}
- [ ] The build has no warnings the project did not already have; the gate, run whole, is green on both platforms; the README current {#gate-green-readme-current}
- [ ] Nothing changed outside the paths the task scopes {#within-scope}
- [ ] Each acceptance item's evidence is a command and what it printed; a screen the task asks to be seen is a screenshot from the simulator or emulator (`xcrun simctl io <device> screenshot`, `adb exec-out screencap -p`) named in the pull request {#evidence-per-item}

## Never shipped by you

You do not archive, build a release bundle, sign for a device or a store,
upload, tag or release. A change ready to ship says so in its pull request
and stops; the warden reviews and lands.
