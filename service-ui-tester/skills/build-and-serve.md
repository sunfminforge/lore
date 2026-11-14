# Building and serving the service

Reach for this before the walk: the service must be built from the task's
branch and answer on its fixtures.

- Record what you build: the branch, the commit (`git rev-parse HEAD`), and the fixture path the service reads; they head the note. {#record-the-build}
- Build the cargo workspace the lockfile's way, `cargo build --locked` (add `--release` or `-p <crate>` only where the repo's docs say), with `CARGO_TARGET_DIR` in your scratch space so the build writes nothing in the repo. A build that needs the lockfile changed is a finding, not a fix. {#cargo-locked}
- Serve it the way the repo documents (its README, its CLI's `--help`, a `serve` subcommand), on its committed fixtures, bound to 127.0.0.1 on a free port; never a live service, never a token of your own. Where the docs say nothing on fixtures, ask the foreman. {#serve-on-fixtures}
- Start the server in the background with its log in scratch space, and wait until it answers (poll the page with `curl -s -o /dev/null -w '%{http_code}'`, at most a minute), never a fixed sleep. Done when every page the task names gives a status code. {#wait-until-it-answers}
- A service that will not build or start is a finding: the command, its exit code and the last lines of its log, in the note; then stop and hand that in. Change no source, flag or fixture to make it start. {#failure-is-a-finding}
- When you hand in, stop the server you started (kill its process, check the port is free); the target directory stays in scratch. {#stop-the-server}
