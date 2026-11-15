# Writing SwiftUI

Reach for this when you write or change a view. Its rules are adapted from
Paul Hudson's swiftui-pro skill (MIT), whose commit the adopting pull
request pins; the repo's own conventions outrank them.

## Structure

- A view body is layout: a button's action is a method of its model, business logic never sits inline in `body`, `task()` or `onAppear()`. {#body-is-layout}
- A part of a body worth naming is its own `View` struct in its own file, not a computed property or method returning `some View`. {#subview-struct}
- `#Preview` for every screen, fed by the in-memory store with sample data made for the app. {#preview-on-sample-data}

## State and data flow

- `@State` a view owns is `private` and initialised in one place; shared state is an `@Observable` class, never `ObservableObject`, `@Published`, `@StateObject` or `@EnvironmentObject` in new code. {#modern-state}
- Bind to `@State`, `@Binding` or `@Bindable` projections, not `Binding(get:set:)` in `body`; a number is entered into a `TextField(value:format:)` with the matching keyboard type. {#projected-bindings}
- A row's identity is the element's stable `id` (`Identifiable`), allocated when the element is stored, never an index, an offset or an editable field. {#stable-identity}

## Modern API

- `NavigationStack` with a typed path, `foregroundStyle`, `.animation(_:value:)`, `Task.sleep(for:)`, `async`/`await` over completion handlers and actors over Dispatch, `FormatStyle` (`.formatted()`) over format strings, `localizedStandardContains()` for filtering what a user typed. {#modern-api}
- No force unwrap or force `try` where failure can happen; an error from a user's action is shown to them, never only printed. {#no-silent-failure}
- Strict concurrency holds: no mutable shared state outside an actor or the main actor, no warning left in the build. {#strict-concurrency}

## Every reader

- Every control has a label VoiceOver can say (`Button("Add Item", systemImage: "plus", action: add)`); an image that carries meaning has an accessibility label, one that does not is hidden from it. {#labelled-controls}
- Text uses Dynamic Type styles, never fixed sizes; the layout holds at the largest accessibility size and at the smallest iPhone width. {#dynamic-type}
- Colours are semantic or from the asset catalog with light and dark variants; the app reads right in both. {#light-and-dark}
- User-visible text is a localizable string (`Text("…")`, `String(localized:)`), in the app's own words; icons are SF Symbols or art made for the app. {#own-words-and-art}
