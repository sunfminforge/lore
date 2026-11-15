# Walking the app in the simulator

## The matrix

- List every screen or flow step the task names, numbered in the order you walk them. For each, the devices (when the task names none, one current iPhone on the newest installed runtime, and say that you chose it) and both appearances, `light` and `dark`, unless the task names one. {#list-steps-devices-appearances}
- One screenshot per cell: step × device × appearance, plus each state the task cares about (empty, loading, error, long content, signed out, a larger Dynamic Type size). {#one-shot-per-cell}
- Name each file so it says its cell: `<nn>-<step>-<device>-<appearance>[-<state>].png`. {#file-names-its-cell}

## A device of your own

- Create a fresh device per device type, named for the task (`xcrun simctl create <task>-<device> <device type> <runtime>`; `xcrun simctl list devicetypes runtimes` names them), and address it by the UDID create prints, never `booted`, which may be someone else's. A missing runtime is a question to the foreman; install none. {#create-own-device}
- Boot it and wait until it is up (`xcrun simctl bootstatus <udid> -b`), never a fixed sleep. {#boot-and-wait}
- Fix what would differ between runs before the first shot: `xcrun simctl status_bar <udid> override --time 9:41 --batteryState charged --batteryLevel 100 --cellularBars 4 --wifiBars 3`, and the locale the task names, English otherwise. {#fix-the-status-bar}
- Set the appearance with `xcrun simctl ui <udid> appearance light|dark`, then relaunch the app (`xcrun simctl terminate`, `launch`) so every screen draws in it; Dynamic Type with `xcrun simctl ui <udid> content_size <size>`. {#set-the-appearance}
- Install and launch the built app (`xcrun simctl install <udid> <App>.app`, `xcrun simctl launch <udid> <bundle id>`), and capture what the app says beside what it shows: its log while you walk (`xcrun simctl spawn <udid> log stream --predicate 'subsystem == "<bundle id>"'` into scratch), and whether it crashed (a launch that returns no pid, or a crash report in `~/Library/Logs/DiagnosticReports` named for the app). {#install-launch-capture}

## Reaching each screen

- Shoot with `xcrun simctl io <udid> screenshot <file>` once the screen is settled: its content drawn, no spinner, animations ended; wait on the app's log or take the shot again until two in a row match, never a fixed sleep alone. {#shoot-when-settled}
- Reach a screen the way the app already offers, read-only: a deep link it declares (`xcrun simctl openurl <udid> <url>`), a launch argument or environment it documents (`simctl launch <udid> <id> -<arg>`), or its own XCUITest target run against your device (`xcodebuild test -scheme <scheme> -destination 'platform=iOS Simulator,id=<udid>' -only-testing:<target>/<test> -resultBundlePath <scratch>/r.xcresult`, its screenshots exported with `xcrun xcresulttool export attachments`). {#reach-the-app-s-way}
- A screen none of these reach is named as not seen, and how you tried; a UI test the app lacks is not yours to write, so where the task needs that screen, ask the foreman. {#unreached-is-not-seen}
- Where a step would send, buy, delete or sign in with a real account, stop before it and say so in the note; the simulator walk uses no credential of anyone's. {#stop-before-real-actions}
- When a step fails or the app crashes, keep the shot of where it stopped and the log lines around it; relaunch and go on with the other cells. {#keep-the-failed-step}
