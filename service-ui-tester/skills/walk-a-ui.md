# Walking the pages

## The matrix

- List every page or flow step the task names, numbered in the order you walk them; for each, the widths (375, 768 and 1280 CSS pixels when the task names none, and say you chose them) and the color schemes (`light` and `dark` when it names none). {#list-the-cells}
- One screenshot per cell, step × width × scheme, plus each state the task cares about (empty, error, long content), named `<nn>-<step>-<width>-<scheme>[-<state>].png`. {#one-shot-per-cell}

## Driving the browser

- Drive the system Google Chrome with playwright-core: `npm install playwright-core` in a scratch directory (it downloads no browser), then `chromium.launch({ channel: 'chrome', headless: true })`. Install nothing globally and nothing in the repo; a missing Chrome or node is a question to the foreman. {#playwright-core-system-chrome}
- Reach a width by device emulation, a fresh context per cell with `viewport: { width, height }`, never by window size: headless Chrome will not size a window below about 500px, so `--window-size=375,…` gives a wider page than asked. Check `window.innerWidth` equals the cell's width and note it. {#width-by-emulation}
- Set the scheme on the context (`colorScheme`), with a fixed locale, timezone, device scale factor and `reducedMotion: 'reduce'`, so runs compare; where a page shows the time or a relative date, fix it with `page.clock.setFixedTime(...)` before `goto`, and name that time in the note. {#fix-the-context}
- Wait for the page to settle on what it shows, never on the network and never a fixed sleep: `goto` with the default `load`, then wait for the element the cell is about (`locator.waitFor()`) and `document.fonts.ready`. `networkidle` is discouraged by Playwright and never comes on a page that polls or holds a socket open. {#wait-to-settle}
- Shoot with `animations: 'disabled'` and the mouse moved off the page (`page.mouse.move(-1, -1)`), so a transition or a hover is not caught half done; full-page unless the task asks for the viewport. {#shoot-still}
- Reach each flow step the way a user would, by user-facing locators (`getByRole`, `getByLabel`, `getByText`); CSS or XPath only where the page offers no name, and said so in the note. A step whose element does not appear within the timeout is a failed cell, not a reason to guess another selector. {#step-by-role}
- Capture what the page says beside what it shows: the HTTP status, console errors, failed requests, horizontal overflow at the width (`scrollWidth > clientWidth`), the title, and where words matter, the page's text as its accessibility tree (`page.locator('body').ariaSnapshot()`) beside the shot. {#capture-what-it-says}
- When a cell fails, keep the shot of where it stopped and the error, and go on with the other cells; close the browser when the matrix is done. {#failed-cell-goes-on}
