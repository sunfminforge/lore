# Shaping an app

Reach for this before writing code for a new screen, a new kind of data,
or a change to what the app stores.

## Logic apart from views

- The logic lives in a module that imports no UI framework: a local Swift package and a pure Kotlin module for native, a Dart package or `lib/` folder with no `package:flutter` import for Flutter, `commonMain` for Kotlin Multiplatform. The app targets depend on it and add only views and entry points. A new repo starts this way; an existing one moves logic out only as far as its task needs. {#logic-module}
- With two native apps, the logic is written twice to one spec: the same rules, the same test cases in both, so the two cannot drift; a test case added to one is added to the other in the same change. {#one-spec-two-apps}
- A module earns its place by depth: much deciding behind few public types. Were it gone, would every view re-decide what it decides? If not, fold it in. {#deep-module}
- What varies is passed in: the time as an argument or a clock, randomness as a generator, the store as a value made by its caller. A function that reads the current time itself cannot be tested at midnight. {#pass-in-what-varies}
- A screen's state lives in one model per screen (an `@Observable` class, a `ViewModel` with a `StateFlow`, a `ChangeNotifier` or the like) whose methods are the intents the view sends, each one line of the view's control. {#one-model-per-screen}
- A rule the app keeps (a count never negative, a name never empty) is a type whose only constructor checks it, so nothing past the edge has to re-check. {#rule-as-type}

## Data on the device

- The store is one module's job: the platform's database (SwiftData, Room, SQLite through the chosen way's package) for records the app queries, a file in the app's own storage for a small document, the platform's preferences for a setting. Pick the least that holds the task's data, and say why in an ADR when it is new. {#one-store}
- A write is whole: a file written to a temporary name and renamed, a database change in one transaction, so a kill leaves the old data or the new. {#atomic-write}
- The store opens in memory or in a temporary directory for tests through the same code path the app uses: it is the real store, not a mock. {#in-memory-store}
- Data a user kept survives every update: a changed stored type is a new schema or file version with a migration, and a test opens a store written by the version before. Never change a stored type in place. {#data-survives-updates}
- Nothing leaves the device: no network client, no `INTERNET` permission or network entitlement, no analytics, crash reporter, cloud backup or sync unless the task says so; Android's `allowBackup` is off where children's data is kept. {#nothing-leaves}
- A child's data: keep the fewest fields the feature needs, no real name, school or location unless the task asks, and a way in the app to delete it all; each rule of the research's minors' information section the app meets is named in the README. {#least-child-data}

## The project

- The toolchain versions stay as the project pins them (deployment target and Swift, `minSdk` and Kotlin and AGP, Flutter and Dart); a newer API waits behind an availability check or for the task that raises the pin. {#keep-the-pins}
- Dependency versions are locked and the lock file committed (`Package.resolved`, the Gradle version catalog and wrapper, `pubspec.lock`), so the gate builds the same thing each time. {#locked-dependencies}
- One type per file, files in folders by feature; the Xcode scheme the gate builds is shared and committed. {#one-type-per-file}
- Text the user reads lives in the platform's string resources (a String Catalog, `strings.xml`, ARB files), never inline in a view, so its language is one place. {#strings-in-resources}
