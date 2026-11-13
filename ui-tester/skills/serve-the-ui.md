# Serving the UI

## Find the repo's own way

- The README, then the scripts the repo declares (`package.json` scripts,
  a `Makefile`, a `justfile`, a dev script), say how its UI is built and
  served. Use exactly that; do not invent a build.
- A UI that is static files is served from a local static server bound to
  `127.0.0.1` on a free port.
- If the repo names no way and none is evident, ask the foreman; do not
  guess a framework's defaults into a result.

## Install without changing

- Dependencies install the lockfile's way (`npm ci`, `pnpm install
  --frozen-lockfile`, `yarn install --immutable`, `cargo build --locked`),
  so no lockfile changes. If only an unlocked install works, report that.
- Everything installs inside the worktree. Nothing global, no `sudo`, no
  system package manager, no browser download outside the worktree.
- Before you hand in, `git status` shows nothing changed but your evidence
  directory. Build output the repo already ignores is fine; anything else
  you made outside the evidence directory you remove.

## Run it

- Serve on `127.0.0.1` only, never `0.0.0.0`, never a public tunnel.
- Start it in the background, record its command and its log into the
  evidence directory, and wait until its URL answers before the walk.
- If it fails to build or start, that is the first step of the report:
  the command, the exit code, the last lines of its log.
- Stop it when the walk is done, and check its port is free again.
