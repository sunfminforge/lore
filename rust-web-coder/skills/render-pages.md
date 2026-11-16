# Rendering pages

## Server-side, no build step

- HTML is rendered on the server from a view model the pure crate built, by a template engine checked at compile time. Every value is escaped by the engine; nothing is built by string concatenation. {#server-rendered-html}
- CSS and JS are plain files in the repo, embedded in the binary at compile time (`include_str!`) and served with the page. No npm, no bundler, no framework, nothing fetched from a CDN at run time. {#embedded-css-js}
- JS is a progressive touch: refresh, sort, copy, a relative time. The page is whole and readable with it off. A few dozen lines, not an app. {#js-is-a-progressive-touch}
- Links are real links; filters and pages are query strings, so every view has a URL an agent or a person can share. {#real-links}

## Light and dark

- Colours are CSS custom properties on `:root`, redefined under `@media (prefers-color-scheme: dark)`; `color-scheme: light dark` is set. No colour is written anywhere else. {#colour-tokens}
- Contrast meets WCAG AA in both; state is never shown by colour alone: a word or a symbol carries it too. {#contrast-aa}

## Phone width

- `<meta name="viewport" content="width=device-width, initial-scale=1">`. {#viewport-meta}
- One column by default, wider layouts only by `min-width` media queries. No horizontal page scroll at 360px: a wide table scrolls in its own box, long words and URLs wrap. {#one-column-default}
- Tap targets at least 44px; text at least 16px. {#tap-targets-text-size}

## Read-only and safe

- Only `GET` routes; no form posts, no buttons that change anything. {#get-routes-only}
- Send `Content-Security-Policy` allowing only the page's own scripts and styles, and `X-Content-Type-Options: nosniff`, set once as `tower_http` `SetResponseHeaderLayer`s on the router so every route has them, and asserted by a `oneshot` test. {#security-headers}
- An error page says what could not be read and what to run (`check`), never a stack trace, a URL with a secret, or the token. {#error-page}

## Before you hand in

Open each page in a browser at desktop width and at 360px, in light and
dark, with JS on and off, and say in the pull request that you did.
