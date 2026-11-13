# Handing in evidence

## Look first

Open every screenshot before you hand it in. A shot of a blank page, a
spinner, a sign-in wall or an error page is evidence of that, not of the
page asked for: say what it shows, and walk again if the walk was wrong.

## The note

One entry per cell, in the matrix's order:

- **cell**: page, URL (secrets stripped), width, scheme, state;
- **of**: the build or fixture, or the live service and the time;
- **showed**: what is on the page in plain words: what renders, what is
  missing, what overlaps, overflows, is cut off or unreadable in the
  scheme; console errors and failed requests;
- **file**: the screenshot's name.

Then, apart, what was not seen and why (a page that would not load, a step
not taken because it would change data, a cell out of budget), and each
acceptance item with the cells that bear on it. You say what was seen; you
do not say the work passes.

## No secret in evidence

Before handing in, for every shot, URL, log line and the note:

- strip query strings and fragments that carry tokens, codes or session
  ids; never put a credential in a filename;
- mask on the page before the shot (Playwright's `mask` option, or an
  injected style hiding the element) any field showing a token, key,
  password, cookie, email, account number or other private data; masking
  afterwards in an image editor is not enough;
- never shot browser storage, devtools or the environment;
- when in doubt, leave the shot out and say why in the note.

## Where it goes

Put the screenshots and the note where the task says. When the task
scopes a path for evidence, that path only; never beside the UI's code.
When it says nothing, ask the foreman before you hand in. Mark each
acceptance item with `guild task check <task> <item> --evidence "<the
cells and what they showed>"`. Then stop: the warden judges.
