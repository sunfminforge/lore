# Shaping an app

Reach for this before writing code for a new screen, a new kind of data,
or a change to what the app stores.

## Logic apart from views

- The logic lives in a local Swift package (or framework target) of the repo that imports Foundation, never SwiftUI or UIKit; the app target depends on it and adds only views and the app's entry point. A new repo starts this way; an existing one moves logic out only as far as its task needs. {#logic-module}
- A module earns its place by depth: much deciding behind few public types, whose interface is all a caller must know. Were it gone, would every view re-decide what it decides? If not, fold it in. {#deep-module}
- What varies is passed in: the date as an argument or a clock, randomness as a generator, the store as a value made by its caller. A function that reads `Date.now` itself cannot be tested at midnight. {#pass-in-what-varies}
- A view's model is an `@Observable` class marked `@MainActor`, owned with `@State` at the screen that creates it and passed down by `@Bindable` or `@Environment`; its methods are the intents the view sends, each one line of the view's button or gesture. {#observable-model}
- A rule the app keeps (a count never negative, a name never empty) is a type whose only initializer checks it, so nothing past the edge has to re-check. {#rule-as-type}

## Data on the device

- The store is one module's job: SwiftData for records the app queries, a `Codable` file in Application Support for a small document, `@AppStorage` for a setting. Pick the least that holds the task's data, and say why in a decision record when it is new. {#one-store}
- A write is whole: a file is written with `.atomic`, a SwiftData change saved in one context save, so a kill leaves the old data or the new. {#atomic-write}
- The store opens in memory for tests and previews (`ModelConfiguration(isStoredInMemoryOnly: true)`, a temporary directory) through the same code path the app uses: it is the real store, not a mock. {#in-memory-store}
- Data a user kept survives every update: a changed model is a new schema version with a migration (`VersionedSchema`, `SchemaMigrationPlan`, or a file format version read by the old and new reader), and a test opens a store written by the version before. Never change a stored type in place. {#data-survives-updates}
- Nothing leaves the device: no `URLSession`, no network entitlement, no analytics or crash reporter, no iCloud or CloudKit container unless the task says so. {#nothing-leaves}

## The project

- A new project's app target uses folder-synchronized groups, so a new Swift file needs no edit of the project file; the project file changes only for a target, a setting or a scheme, and is committed with the change that needs it. {#synchronized-groups}
- One type per file, files in folders by feature; the shared scheme the gate builds is committed, and its test action lists every test target the gate must run, a local package's too (`container:<PackageDir>`), since xcodebuild runs only the tests its scheme names. {#one-type-per-file}
- The deployment target and Swift version stay as the project has them; a newer API goes behind `#available` or waits for the task that raises the target. {#keep-the-target}
