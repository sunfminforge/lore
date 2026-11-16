# Walking the pages

## The matrix

- List every page or flow step the task names, numbered in the order you walk them; for each, the widths (375, 768 and 1280 CSS pixels when the task names none, and say you chose them) and the color schemes (`light` and `dark` when it names none). {#list-the-cells}
- One screenshot per cell, step × width × scheme, plus each state the task cares about (empty, error, long content), named `<nn>-<step>-<width>-<scheme>[-<state>].png`. {#one-shot-per-cell}

## Driving the browser

- Drive the system Google Chrome with playwright-core: `npm install playwright-core` in a scratch directory (it downloads no browser), then `chromium.launch({ channel: 'chrome', headless: true })`. Install nothing globally and nothing in the repo; a missing Chrome or node is a question to the foreman. {#playwright-core-system-chrome}
- Reach a width by device emulation, a fresh context per cell with `viewport: { width, height }`, never by window size: headless Chrome will not size a window below about 500px, so `--window-size=375,…` gives a wider page than asked. Check `window.innerWidth` equals the cell's width and note it. {#width-by-emulation}
- Set the scheme on the context (`colorScheme`), with a fixed locale, timezone, device scale factor and `reducedMotion: 'reduce'`, so runs compare; where the page computes times in the browser, fix its clock too (`page.clock.setFixedTime`) and note the time. {#fix-the-context}
- Wait for the page to settle on what the step expects: an element of it visible, then `document.fonts.ready`; never a fixed sleep, and `networkidle` only as a fallback, which Playwright discourages. Shoot with `animations: 'disabled'` and the mouse moved off the page, full-page unless the task asks for the viewport. {#wait-to-settle}
- Drive a flow step by what the page shows: look first (a shot or `page.content()`), then act with locators by role or visible text read off it, never a guessed CSS path. {#look-then-act}
- Capture what the page says beside what it shows: the HTTP status, console errors, failed requests, horizontal overflow at the width (`scrollWidth > clientWidth`), and the title. {#capture-what-it-says}
- When a cell fails, keep the shot of where it stopped and the error, and go on with the other cells; close the browser when the matrix is done. {#failed-cell-goes-on}
