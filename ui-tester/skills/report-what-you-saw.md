# Reporting what you saw

## The report

`REPORT.md` in the evidence directory:

- What was walked: the repo and commit, the command that served it, the
  browser and its version, the four views, the date.
- One section per step, in order: what the step did, then for each view
  what happened, its screenshot or recording, and any console errors,
  page errors or failed requests, quoted.
- Failures are reported as plainly as successes and in the same place: the
  step, the view, what was expected to be acted on, what happened instead,
  the error text.
- Differences between views are stated as observations: "on phone-dark
  the menu button is not in the screenshot", not "phone layout is broken".
- A step not walked says why.

## What it never says

- No verdict: no "passes", "fails", "looks good", "broken", "should".
  Describe; the reader judges.
- No suggestion of a fix, no guess at the cause beyond the error text the
  browser gave.

## Handing in

- `git status` shows changes only in the evidence directory; every server
  and browser you started is stopped.
- Open the pull request in the repo the task changes, with `Closes
  <task>` in its body and a short list of the steps and where each one's
  evidence lies.
- Mark an acceptance item only where it asks for evidence to exist (a
  walk recorded, a report written), with `guild task check <task> <item>
  --evidence "<what shows it>"`. An item that asks whether the UI is
  right is left for whoever judges.
- Then stop: the warden reviews and lands.
