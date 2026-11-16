# Building the debug APK

Reach for this before the walk: the app must be built from the task's
commit, debug variant, outside the repo.

- Record what you build: the branch, the commit (`git rev-parse HEAD`), the module and variant, `./gradlew --version` and the Android Gradle Plugin version the build names; they head the note. {#record-the-build}
- Build from a scratch copy of that commit (`git archive <commit> | tar -x -C <scratch>`), never in the repo, so nothing there changes; `./gradlew projects` and `./gradlew :<module>:tasks --group build` on the copy name the modules and variants, and where the task names none and there are several app modules or flavours, ask the foreman. {#scratch-copy}
- Keep Gradle's state in scratch too: `GRADLE_USER_HOME=<scratch>/gradle-home` unless the machine's shared cache is the task's to use, and `ANDROID_USER_HOME=<scratch>/android-home`, so the debug keystore the build creates and every AVD stay yours. {#state-in-scratch}
- Build the debug variant with the repo's own wrapper: `./gradlew :<module>:assemble<Flavour>Debug 2>&1 | tee <scratch>/build.log`. The debug variant is signed with the SDK's debug key, which the build creates itself; ask for no keystore, password or release signing config. {#debug-variant}
- Resolve dependencies the way the repo commits them: the wrapper's pinned Gradle, the version catalog, and `gradle.lockfile`s where they are committed (never `--write-locks`). A build that needs a build file or lockfile changed is a finding, not a fix. {#locked-dependencies}
- Done when `<module>/build/outputs/apk/<flavour>/debug/*.apk` exists and its application id and launcher activity are read from it (`apkanalyzer manifest application-id <apk>`, or `aapt2 dump badging <apk>` for `package:` and `launchable-activity:`), with its `minSdkVersion` and `targetSdkVersion`, so the AVD's API level can be checked against them. {#apk-exists}
- An app that will not build is a finding: the command, its exit code and the first `FAILURE:` and `e:` or `error:` lines of its log, in the note; then stop and hand that in. Change no source, setting or flag to make it build. {#failure-is-a-finding}
