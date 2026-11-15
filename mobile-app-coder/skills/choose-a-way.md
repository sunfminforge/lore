# Choosing a way to build

Reach for this when a repo has no ADR saying how its app is built, or a
task asks to trial a way: native per platform, or one cross-platform
codebase.

## The trial

- Name the candidates from what the machine has and Guild can get: native (SwiftUI, and Kotlin with Jetpack Compose), Flutter, Kotlin Multiplatform with native or Compose views. Two or three, not every framework there is. {#name-the-candidates}
- Judge them on the task's own needs, said before the trial starts: the screens and platform features the research's feature set asks for, the language of the text, the data kept, how the logic is tested off the device, and what the gate must run on both platforms. {#criteria-first}
- Each trial is a tracer bullet: in your scratch space, the smallest app that has one screen, one logic function with a host test, one stored value read back after a relaunch, built and launched on both the simulator and the emulator. Done when each candidate has done all of that or failed at a step you can name. {#tracer-bullet}
- Measure, not recall: a clean build's time on each platform, the host tests' time, what had to be got (SDKs, packages), and anything that failed and how. What a trial did not try is written as not tried. {#measure-not-recall}
- A trial changes nothing outside scratch space; the repo gets only the ADR and, once chosen, the way's skeleton. {#trial-in-scratch}

## The ADR

- One ADR in the repo's decisions folder, numbered as its others are: context (the needs), the options, what each trial showed with its numbers, the decision, and its consequences: what gets harder, what would make it worth revisiting. {#adr-shape}
- The decision follows from the trial: where two ways tie, the one with fewer toolchains and fewer packages wins. {#fewer-toolchains-win}
- Once landed, the ADR is the authority: a later task changes the way only by a new ADR that supersedes it. {#adr-is-authority}
