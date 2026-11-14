# Handing in evidence

## Look first

Open every screenshot before you hand it in. A shot of a blank page, a
spinner, an error page or the server's 404 is evidence of that, not of the
page asked for: say what it shows, and walk again if the walk was wrong.

## The note

The note opens with the branch, commit and fixture walked, and the time.
Then one entry per cell, in the matrix's order:

- **cell**: page, URL (secrets stripped), width (and the `innerWidth` seen), scheme, state. {#note-cell}
- **showed**: what is on the page in plain words: what renders, what the step expected that is not there, what overlaps, overflows or is cut off, text whose color matches its background; the HTTP status, console errors and failed requests, quoted. {#note-showed}
- **file**: the screenshot's name. {#note-file}

A step that failed is reported in its place, as plainly as one that did
not: what the step did, what happened instead, the error text. No word of
verdict: not "passes", "fails", "broken", "looks good" or "should"; no
guess at the cause beyond the error the browser gave, no suggested fix.

Then, apart, what was not seen and why (a page that would not load, a
build that failed, a cell out of budget), and each acceptance item with
the cells that bear on it.

## No secret in evidence

- Strip query strings and fragments that carry tokens, codes or session ids; never put a credential in a filename. {#strip-urls}
- Mask on the page before the shot (Playwright's `mask` option) any field showing a token, key, password, cookie, email or other private data; leave the shot out and say why when in doubt. {#mask-before-the-shot}
- Never shoot browser storage, devtools or the environment, and quote no server log line that carries a secret. {#never-shoot-storage}

## Where it goes

- Write the screenshots and the note to the evidence path the task names, and nowhere else; when it names none, ask the foreman before you hand in. {#evidence-path}
- Mark an acceptance item only where it asks for evidence to exist, with `guild task check <task> <item> --evidence "<the cells and what they showed>"`; one that asks whether the service is right is left for whoever judges. {#mark-only-evidence-items}
- Done when every server and browser you started is stopped and `git status` in the service's repo shows nothing changed; then stop: the warden judges. {#done-clean}
