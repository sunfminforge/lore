# Handing in evidence

## Look first

Open every screenshot before you hand it in. A shot of the home screen, a
launch screen, a system alert or a crash is evidence of that, not of the
screen asked for: say what it shows, and walk again if the walk was wrong.

## The note

It opens with the build (branch, commit, scheme, Xcode version), then one
entry per cell, in the matrix's order:

- **cell**: step, screen, device, iOS runtime, appearance, state; {#note-cell}
- **of**: the commit, and the time of the shot; {#note-of}
- **showed**: what is on the screen in plain words: what renders, what the step expected that is not there, what overlaps, is cut off or truncated, text whose color matches its background in either appearance, what did not change between light and dark; cells that came out byte-identical (`md5`) where they were to differ, and cells that differ by a clock alone; log errors and crashes, quoted; {#note-showed}
- **file**: the screenshot's name. {#note-file}

A step that failed is reported in its place, as plainly as one that did
not: what the step did, what happened instead, the error or crash text. No
word of verdict: not "passes", "fails", "broken", "looks good" or
"should"; no guess at the cause beyond what the log said, no suggested
fix.

Then, apart, what was not seen and why (a screen no way reached, a step
not taken because it would act for real, a cell out of budget), and each
acceptance item with the cells that bear on it.

## No secret in evidence

Before handing in, for every shot, log line and the note:

- quote only log lines that bear on a cell, never a whole log, and drop any carrying a token, key, cookie, signing identity, or a local path with a username; {#quote-only-what-bears}
- never shoot the Keychain, Settings' accounts, or a screen showing a real person's data; {#never-shoot-accounts}
- when in doubt, leave the shot out and say why in the note. {#doubt-leaves-it-out}

## Where it goes, and stopping

- Put the screenshots and the note in the evidence path the task names, that path only, never beside the app's code; when it names none, ask the foreman before you hand in. {#evidence-path}
- Before you transform shots in bulk (downscale, crop), transform one, open it and check its size; downscale with `sips --resampleWidth <px>` alone, since adding `-Z` shrinks it further. {#one-output-first}
- Before you hand in, stop the log streams you started, then shut down and delete every device you created (`xcrun simctl shutdown <udid>`, `xcrun simctl delete <udid>`); done when `xcrun simctl list devices` shows none named for the task and `git status` in the app's repo shows nothing uncommitted. {#clean-up}
- Commit the evidence path alone on your task's branch, push it, and open the pull with `guild tool forge-pull open <owner/repo> --head <branch> --title <title> --body-file <file>`, its body holding `Closes <owner/repo>#<task>`; done when `guild tool forge-pull find <owner/repo>#<task>` lists it. Evidence on no pull is not seen by the warden, and comes back. {#evidence-on-a-pull}
- Mark an acceptance item only where it asks for evidence to exist, with `guild task check <task> <item> --evidence "<the pull, its head sha, the cells and what they showed>"`; one that asks whether the app is right is left for whoever judges. Then stop: the warden judges. {#mark-and-stop}
