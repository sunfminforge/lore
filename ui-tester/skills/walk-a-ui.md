# Walking a UI

## The matrix

- List every page or flow step the task names, numbered in the order you walk them. For each, the widths (default 375, 768 and 1280 CSS pixels when the task names none, and say that you chose them) and the color schemes (`light`, `dark`; both when the task names none). {#list-steps-widths-schemes}
- One screenshot per cell: step × width × scheme, plus each state the task cares about (empty, loading, error, long content, signed out). {#one-shot-per-cell}
- Name each file so it says its cell: `<nn>-<step>-<width>-<scheme>[-<state>].png`. {#file-names-its-cell}
- Where the task asks for a recording, record each width × scheme as one video of the whole walk (Playwright's `recordVideo` on the context), and note at what time each step starts; take the shots as well unless the task says the recording alone. {#record-when-asked}

## Driving the browser

- Use what the repo already uses (its Playwright config, its test server, its fixtures) read-only; otherwise a Playwright script of your own in your scratch space: `npm install playwright-core` there (it downloads no browser) and launch the system Google Chrome (`channel: 'chrome'`). Install nothing outside your worktree or scratch space; a missing Chrome or node is a question to the foreman. {#use-the-repos-tools}
- Headless, a fresh context per cell: set `viewport` to the width, `colorScheme` to the scheme, `reducedMotion: 'reduce'`, and a fixed locale, timezone and device scale factor so runs compare. {#fresh-context-per-cell}
- Act and wait by what the user sees: find each control by its role and name (`getByRole`), read off the page's aria snapshot first, never by the DOM's structure; after `load`, wait on the content the step expects (`locator.waitFor()`, which retries until it shows) and `document.fonts.ready`. Never `networkidle`, which Playwright discourages and which a page holding a connection open (server-sent events, a websocket, live reload) never reaches, and never a fixed sleep. Shoot full page unless the task asks for the viewport, with `animations: 'disabled'`. {#wait-until-settled}
- Capture what the page says beside what it shows, by the script in each cell: its aria snapshot (`page.ariaSnapshot()`, saved beside the shot as `<shot name>.aria.yml`), console errors, failed requests, the HTTP status, horizontal overflow at the width (`scrollWidth > clientWidth`), the links' hrefs, and the page title. {#capture-page-signals}
- When a step fails, keep the shot of where it stopped and the error; go on with the other cells. {#keep-the-failed-step}

## Fixtures and live services

- On fixtures, record which fixture and which build (commit, or version) you walked: the one the task names, a pull request's head, not an earlier build; shots of an older build are walked again. Build from a scratch copy of that commit (`git archive <commit> | tar -x -C <scratch>`), its dependencies installed the lockfile's way (`npm ci`, `cargo build --locked`), and serve it the way the repo documents, on `127.0.0.1` only; where the walk needs a source edited (a live-reload serve), edit the copy. Change nothing to make it start: a UI that will not build or start is a finding, the command, its exit code and the last lines of its log. {#fixtures-walk-the-named-build}
- On a live service, record its URL and the time. Use only the read-only credential you were handed, passed by the environment, never written in a script, a URL or a file. Take no step that changes data: no submit, save, delete, send, purchase or settings change. Where the task asks for a flow past such a step, stop before it and say so. {#live-changes-no-data}
- Prefer a session that cannot write: a read-only account or token, or the signed-out view. If the credential you were given could write, say so in the note and touch only what reads. {#prefer-a-read-only-session}
