# Handing in evidence

## Look first

Open every screenshot before you hand it in. A shot of a blank page, a
spinner, a sign-in wall or an error page is evidence of that, not of the
page asked for: say what it shows, and walk again if the walk was wrong.

## The note

One entry per cell, in the matrix's order:

- **cell**: page, URL (secrets stripped), width, scheme, state; {#note-cell}
- **of**: the build or fixture, or the live service and the time; {#note-of}
- **showed**: what is on the page in plain words: what renders, what the step expected that is not there, what overlaps, overflows or is cut off, text whose color matches its background; console errors and failed requests, quoted; {#note-showed}
- **file**: the screenshot's name, or the recording's and the time. {#note-file}

A step that failed is reported in its place, as plainly as one that did
not: what the step did, what happened instead, the error text. No word of
verdict: not "passes", "fails", "broken", "looks good" or "should"; no
guess at the cause beyond the error the browser gave, no suggested fix.

Then, apart, what was not seen and why (a page that would not load, a step
not taken because it would change data, a cell out of budget), and each
acceptance item with the cells that bear on it. You say what was seen; you
do not say the work passes.

Before you hand in, stop every server and browser you started, and
`git status` in the UI's repo shows nothing changed.

## No secret in evidence

Before handing in, for every shot, URL, log line and the note:

- strip query strings and fragments that carry tokens, codes or session ids; never put a credential in a filename; {#strip-secret-urls}
- mask on the page before the shot (Playwright's `mask` option, or an injected style hiding the element) any field showing a token, key, password, cookie, email, account number, a local path carrying a username, or other private data; masking afterwards in an image editor is not enough; {#mask-before-the-shot}
- never shot browser storage, devtools or the environment; {#never-shoot-storage}
- when in doubt, leave the shot out and say why in the note. {#doubt-leaves-it-out}

## Where it goes

Put the screenshots and the note where the task says. When the task
scopes a path for evidence, that path only; never beside the UI's code.
When it says nothing, ask the foreman before you hand in. Mark an
acceptance item only where it asks for evidence to exist, with `guild
task check <task> <item> --evidence "<the cells and what they showed>"`;
one that asks whether the UI is right is left for whoever judges. Then stop: the warden judges.
