# Walking a UI

## The matrix

- List every page or flow step the task names, numbered in the order you
  walk them. For each, the widths
  (default 375, 768 and 1280 CSS pixels when the task names none, and say
  that you chose them) and the color schemes (`light`, `dark`; both when
  the task names none).
- One screenshot per cell: step × width × scheme, plus each state the task
  cares about (empty, loading, error, long content, signed out).
- Name each file so it says its cell:
  `<nn>-<step>-<width>-<scheme>[-<state>].png`.
- Where the task asks for a recording, record each width × scheme as one
  video of the whole walk (Playwright's `recordVideo` on the context), and
  note at what time each step starts; take the shots as well unless the
  task says the recording alone.

## Driving the browser

- Use what the repo already uses (its Playwright config, its test server,
  its fixtures) read-only; otherwise a Playwright script of your own in
  your scratch space, never in the repo. Install nothing outside your
  worktree or scratch space; a missing browser or tool is a question to
  the foreman.
- Headless, a fresh context per cell: set `viewport` to the width,
  `colorScheme` to the scheme (`emulateMedia`), and a fixed locale,
  timezone and device scale factor so runs compare.
- Wait for the page to be settled (network idle, fonts loaded, animations
  off with `reducedMotion: 'reduce'`), never a fixed sleep. Take full-page
  shots unless the task asks for the viewport.
- Capture what the page says beside what it shows: console errors, failed
  requests, the HTTP status, horizontal overflow at the width
  (`scrollWidth > clientWidth`), and the page title.
- When a step fails, keep the shot of where it stopped and the error; go on
  with the other cells.

## Fixtures and live services

- On fixtures, record which fixture and which build (commit, or version)
  you walked. Build and serve the app the way the repo documents, its
  dependencies installed the lockfile's way (`npm ci`, `pnpm install
  --frozen-lockfile`), on `127.0.0.1` only; change nothing to make it
  start. A UI that will not build or start is a finding: the command, its
  exit code and the last lines of its log.
- On a live service, record its URL and the time. Use only the read-only
  credential you were handed, passed by the environment, never written in
  a script, a URL or a file. Take no step that changes data: no submit,
  save, delete, send, purchase or settings change. Where the task asks for
  a flow past such a step, stop before it and say so.
- Prefer a session that cannot write: a read-only account or token, or the
  signed-out view. If the credential you were given could write, say so
  in the note and touch only what reads.
