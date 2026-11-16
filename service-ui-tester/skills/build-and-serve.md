# Building and serving the service

Reach for this before the walk: the service must be built from the task's
branch and answer on its fixtures.

- Record what you build: the branch, the commit (`git rev-parse <branch>` after a fetch), and the fixture path the service reads; they head the note. {#record-the-build}
- Build from a copy of that commit in scratch space, `git archive <commit> | tar -x -C <scratch>/src`, never a checkout or switch in the repo you were handed: its branch and tree stay as they were. Build and serve from the copy with its default `target/`, so paths the repo's docs and fixtures assume (`target/…`, `../target/…`) hold as written; a path the docs need that is missing from the copy was `export-ignore`d, and is said in the note. {#build-from-a-copy}
- Build the cargo workspace the lockfile's way, `cargo build --locked` (add `--release` or `-p <crate>` only where the repo's docs say). A build that needs the lockfile changed is a finding, not a fix. {#cargo-locked}
- Serve it the way the repo documents (its README, its CLI's `--help`, a `serve` subcommand), on its committed fixtures, bound to 127.0.0.1 on a free port; never a live service, never a token of your own. Where the docs say nothing on fixtures, ask the foreman. {#serve-on-fixtures}
- Start the server in the background with its log in scratch space, and wait until it answers (poll the page with `curl -s -o /dev/null -w '%{http_code}'`, at most a minute), never a fixed sleep. Done when every page the task names gives a status code. {#wait-until-it-answers}
- A service that will not build or start is a finding: the command, its exit code and the last lines of its log, in the note; then stop and hand that in. Change no source, flag or fixture to make it start. {#failure-is-a-finding}
- When you hand in, stop the server you started (kill its process, check the port is free); the copy and its build stay in scratch. {#stop-the-server}
