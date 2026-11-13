# Walking the UI

## The browser

- A real browser engine, driven headless: Playwright with the browsers it
  installed into the worktree, or the tooling the task or repo names. A
  page fetched as text or rendered by a non-browser is not a walk.
- If no browser tooling can be had inside the worktree, ask the foreman;
  do not fall back to something that is not a browser.

## The four views

Run every step in each:

| view          | viewport  | scale | touch | colour scheme |
|---------------|-----------|-------|-------|---------------|
| desktop-light | 1440×900  | 1     | no    | light         |
| desktop-dark  | 1440×900  | 1     | no    | dark          |
| phone-light   | 390×844   | 3     | yes   | light         |
| phone-dark    | 390×844   | 3     | yes   | dark          |

The colour scheme is the browser's emulated `prefers-color-scheme`, not a
toggle in the app; if the app has its own toggle and the task asks for
it, that is a step of its own. Use the task's sizes where it names others.

## The walk script

- One script (`walk.mjs`, or the tooling's own form) in the evidence
  directory: the steps as the task lists them, in order, named, each
  with what it does (go to, click, type, wait for).
- It finds elements the way a person would: by role and visible text or
  label, not by brittle CSS paths.
- Each step waits for something visible to settle on (a heading, a
  response, a fixed timeout as a last resort, noted as such), then
  records.
- A step that fails is recorded where it failed (screenshot of the page
  as it stood, the error) and the walk goes on to the next step if it
  can; it does not stop at the first failure, and never retries until it
  passes.

## What each step records

- A full-page screenshot per view: `<nn>-<step>-<view>.png`, `nn` the
  step's number.
- Or, where the task asks for one, a recording per view, the step
  boundaries noted by time in the report.
- The browser console messages, page errors and failed network requests
  during the step, per view, as text.
- The URL it ended on.

Use test data only: what the task or the repo's fixtures give. Type no
real account, password or personal data into a form.
