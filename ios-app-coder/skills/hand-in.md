# Handing the work in

## Green gate

The repo's `gate.sh` is the definition of "may land": it builds the app and
runs its tests on the simulator it names. Run it whole, as it is, before you
hand in; a part run by hand is not the gate. Its scripts are committed
executable (`100755`). When it fails for something outside your scope (an
Xcode or simulator runtime missing on the machine, a test another change
broke), ask the foreman rather than work around it.

## README current

The README says what the app is, what it keeps on the device, the Xcode it
needs, and how to run the gate and the app in the simulator. When any of
these changes, the README changes in the same pull request.

## Before you hand in

- [ ] The logic module imports no SwiftUI or UIKit, and every behaviour the task adds has a test on it, seen red before green {#logic-tested-apart}
- [ ] A changed stored type has a migration and a test that opens the version before {#migration-tested}
- [ ] A search of the sources for `URLSession`, `Network`, `CloudKit` and `StoreKit` finds nothing the task did not ask for {#nothing-leaves-the-device}
- [ ] No name, icon, text, art or data of another app is in the repo {#nothing-copied}
- [ ] The build has no warnings; the gate, run whole, is green; the README current {#gate-green-readme-current}
- [ ] Nothing changed outside the paths the task scopes {#within-scope}
- [ ] Each acceptance item's evidence is a command and what it printed; a screen the task asks to be seen is a simulator screenshot (`xcrun simctl io <device> screenshot`) named in the pull request {#evidence-per-item}

## Never shipped by you

You do not archive, export, sign for a device, upload, tag or release. A
change ready to ship says so in its pull request and stops; the warden
reviews and lands.
