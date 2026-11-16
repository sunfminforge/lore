# Shaping a read-only service

## The workspace

One cargo workspace.

- **Pure crates**, as many as the domain has parts: the API's answers as plain types, what is computed from them, the view model each page and each CLI answer is built from. No I/O: the clock, the environment and what was read arrive as arguments. `publish = false`. A crate is pure or it is not. {#pure-crates}
- **One effect crate**, the binary. It reads the environment and the APIs, hands what it read to the pure crates, and serves or prints what they return. `main.rs` is one line calling `main(args) -> ExitCode`. {#one-effect-crate}
- Dependencies stay few: `axum`, `tokio` and `tower-http` for serving, one HTTP client (`reqwest` with rustls), `clap` (derive, env), `serde`, `serde_json`, `anyhow`, a compile-time-checked template engine (`askama` or `maud`); `wiremock` and `insta` only as dev-dependencies. Add one only when the task cannot be done without it, and say why in the pull request. {#few-dependencies}

When a change would put I/O in a pure crate, move the reading into the
effect crate and pass what it read in as an argument.

## Deep view models

- Judge a pure crate by what its handlers are spared: a view model that hands each page and each CLI answer one finished value is deep; one that passes the API's fields through one by one is shallow. Its interface is everything a handler must know, its failed-read values and the order of its calls included. {#judge-by-what-handlers-are-spared}
- Delete it in your head: if what it computes would come back in each handler, it earns its place; if nothing comes back, merge it into its caller. {#delete-it-in-your-head}
- Where a crate's interface is open, draft two unlike ones (the fewest functions; the busiest page's call made trivial) and keep the one that leaves the handlers smallest. {#draft-two-unlike-interfaces}

## Reading APIs

- One module per API (Forgejo, guildd, …), the only place that builds its URLs and sends its requests. Each returns the pure crate's types, never the client's. Its seam is the base URL, filled by the real API when served and by the test's stand-in server when tested, so no client trait or mock sits between. {#one-module-per-api}
- Only `GET` (and `HEAD`). A method that writes is never built, so a token that could write still cannot be used to. {#get-and-head-only}
- The base URL and the token come from the environment at run time, read once in `main` and passed down; `clap`'s `env` is the one place each variable is named. Neither has a default that points at a real system. {#config-from-env}
- The token lives in a type whose `Debug` and `Display` print `***`; it is sent only as the `Authorization` header, never in a URL, and never reaches a log line, an error message, a page or a JSON answer. {#token-type-redacts}
- A failed read becomes a value the pure crate decides what to do with: the page shows what it could read and names what it could not. {#failed-read-is-a-value}
- One `reqwest::Client`, built once in `main` and passed down, since it holds the connection pool; it sets `timeout` and `connect_timeout`, and the router a `tower_http` `TimeoutLayer`, so a page answers in bounded time even when an API hangs. {#timeouts}
- The serving path never blocks the runtime: async reads only, never `reqwest::blocking`, `std::thread::sleep` or a blocking file read in a handler; a heavy pure computation goes through `tokio::task::spawn_blocking`; no lock guard is held across an `.await`. {#never-block-the-runtime}
- A refreshed cache's age counts from when its read began, so a read slower than that age lands already stale: start the next refresh as one lands, and let the first page wait for a fresh one. Refresh an expensive read only when a cheap marker of it moves (a branch head, an ETag). {#cache-age-from-read-start}

## The CLI front

- `serve`: binds the address it is given (default loopback), prints one JSON object naming the URL it serves, then serves until stopped. {#serve-command}
- `check`: reads everything `serve` would, renders nothing, and prints one JSON object: each source reached or not, each finding with the line that fixes it (the variable to set, the URL to reach). Exit non-zero when any source fails. It changes nothing. {#check-command}
- Every command prints exactly one JSON object on stdout; warnings go to stderr; a failure is `{"error": "…"}` with a non-zero exit, naming every fault at once. {#one-json-object}
- `--help` is the manual: what the service is, what it needs (each variable by name, never its value), what to run first. Where the repo uses `agent-help`, help is written with it. {#help-is-the-manual}
- `--version` stamped by `build.rs`. {#version-from-build-rs}

## Nothing in the process

No database, no cache that is the truth, no file only this service
understands. A cache, if the task asks for one, is short-lived, bounded,
and losing it loses nothing.

## Words and decisions

New terms go in the glossary, decisions in the repo's decision records,
module and interface comments first, before the code they describe.
