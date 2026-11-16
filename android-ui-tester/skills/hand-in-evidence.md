# Handing in evidence

## Look first

Open every screenshot before you hand it in. A shot of the launcher, a
splash screen, a system dialog ("isn't responding", a permission prompt)
or a crash is evidence of that, not of the screen asked for: say what it
shows, and walk again if the walk was wrong.

## The note

It opens with the build (branch, commit, module, variant, Gradle and
Android Gradle Plugin versions), then one entry per cell, in the matrix's
order:

- **cell**: step, screen, AVD and its device profile, API level and system image, appearance, state; {#note-cell}
- **of**: the commit, and the time of the shot; {#note-of}
- **showed**: what is on the screen in plain words: what renders, what the step expected that is not there, what overlaps, is cut off or truncated, text whose color matches its background in either appearance, what did not change between light and dark; log errors, crashes and ANRs, quoted; {#note-showed}
- **file**: the screenshot's name. {#note-file}

A step that failed is reported in its place, as plainly as one that did
not: what the step did, what happened instead, the error or crash text. No
word of verdict: not "passes", "fails", "broken", "looks good" or
"should"; no guess at the cause beyond what the log said, no suggested
fix.

Then, apart, what was not seen and why (a screen no way reached, a step
not taken because it would act for real, an AVD whose system image is not
installed, a cell out of budget), and each acceptance item with the cells
that bear on it.

## No secret in evidence

Before handing in, for every shot, log line and the note:

- quote only logcat lines that bear on a cell, never a whole log, and drop any carrying a token, key, cookie, keystore path or password, or a local path with a username; {#quote-only-what-bears}
- never shoot the device's accounts settings, a keystore prompt, or a screen showing a real person's data; {#never-shoot-accounts}
- when in doubt, leave the shot out and say why in the note. {#doubt-leaves-it-out}

## Where it goes, and stopping

- Put the screenshots and the note in the evidence path the task names, that path only, never beside the app's code; when it names none, ask the foreman before you hand in. {#evidence-path}
- Before you hand in, stop the logcat you started, then stop and delete every emulator and AVD you created (`adb -s emulator-<port> emu kill`, `avdmanager delete avd -n <name>` with your `ANDROID_AVD_HOME`), and stop your own adb server (`adb -P <your port> kill-server`); done when `emulator -list-avds` with your `ANDROID_AVD_HOME` shows none, `pgrep -f <avd name>` finds nothing, and `git status` in the app's repo shows nothing changed. {#clean-up}
- Mark an acceptance item only where it asks for evidence to exist, with `guild task check <task> <item> --evidence "<the cells and what they showed>"`; one that asks whether the app is right is left for whoever judges. Then stop: the warden judges. {#mark-and-stop}
