# Building for the simulator

Reach for this before the walk: the app must be built from the task's
commit, for the simulator, outside the repo.

- Record what you build: the branch, the commit (`git rev-parse HEAD`), the project or workspace, the scheme and configuration, and `xcodebuild -version`; they head the note. {#record-the-build}
- Build from a scratch copy of that commit (`git archive <commit> | tar -x -C <scratch>`), never in the repo, so nothing there changes; `xcodebuild -list` on the copy names the schemes, and where the task names none and there are several, ask the foreman. {#scratch-copy}
- Build unsigned for the simulator into derived data in scratch: `xcodebuild build -scheme <scheme> -destination 'generic/platform=iOS Simulator' -derivedDataPath <scratch>/dd CODE_SIGNING_ALLOWED=NO`, adding `-workspace` or `-project` as the copy needs. A simulator build needs no signing identity, team or provisioning profile; ask for none. {#unsigned-build}
- Resolve dependencies the lockfile's way: Swift packages pinned by the committed `Package.resolved` (`-onlyUsePackageVersionsFromResolvedFile`), CocoaPods by `pod install --deployment` in the copy where a `Podfile.lock` is committed. A build that needs a lockfile changed is a finding, not a fix. {#locked-dependencies}
- Keep the build log in scratch (`2>&1 | tee <scratch>/build.log`); done when `<scratch>/dd/Build/Products/*-iphonesimulator/<App>.app` exists, and its bundle id read from its `Info.plist` (`/usr/libexec/PlistBuddy -c 'Print :CFBundleIdentifier'`). {#app-exists}
- An app that will not build is a finding: the command, its exit code and the first `error:` lines of its log, in the note; then stop and hand that in. Change no source, setting or flag to make it build. {#failure-is-a-finding}
