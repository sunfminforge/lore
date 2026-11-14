# Shaping a read-only service

## The workspace

One cargo workspace.

- **Pure crates**, as many as the domain has parts: the API's answers as plain types, what is computed from them, the view model each page and each CLI answer is built from. No I/O: the clock, the environment and what was read arrive as arguments. `publish = false`. A crate is pure or it is not. {#pure-crates-as-many-as-the}
- **One effect crate**, the binary. It reads the environment and the APIs, hands what it read to the pure crates, and serves or prints what they return. `main.rs` is one line calling `main(args) -> ExitCode`. {#one-effect-crate-the-binary-it}
- Dependencies stay few: `axum` and `tokio` for serving, one HTTP client (`reqwest` with rustls), `clap` (derive, env), `serde`, `serde_json`, `anyhow`, a compile-time-checked template engine (`askama` or `maud`). Add one only when the task cannot be done without it, and say why in the pull request. {#dependencies-stay-few-axum-and}

When a change would put I/O in a pure crate, move the reading into the
effect crate and pass what it read in as an argument.

## Reading APIs

- One module per API (Forgejo, guildd, …), the only place that builds its URLs and sends its requests. Each returns the pure crate's types, never the client's. {#one-module-per-api-forgejo}
- Only `GET` (and `HEAD`). A method that writes is never built, so a token that could write still cannot be used to. {#only-get-and-head-a-method-that}
- The base URL and the token come from the environment at run time, read once in `main` and passed down; `clap`'s `env` is the one place each variable is named. Neither has a default that points at a real system. {#the-base-url-and-the-token-come}
- The token lives in a type whose `Debug` and `Display` print `***`; it is sent only as the `Authorization` header, never in a URL, and never reaches a log line, an error message, a page or a JSON answer. {#the-token-lives-in-a-type-whose}
- A failed read becomes a value the pure crate decides what to do with: the page shows what it could read and names what it could not. {#a-failed-read-becomes-a-value}
- Timeouts on every request; a page answers in bounded time even when an API hangs. {#timeouts-on-every-request-a-page}

## The CLI front

- `serve`: binds the address it is given (default loopback), prints one JSON object naming the URL it serves, then serves until stopped. {#serve-binds-the-address-it-is}
- `check`: reads everything `serve` would, renders nothing, and prints one JSON object: each source reached or not, each finding with the line that fixes it (the variable to set, the URL to reach). Exit non-zero when any source fails. It changes nothing. {#check-reads-everything-serve}
- Every command prints exactly one JSON object on stdout; warnings go to stderr; a failure is `{"error": "…"}` with a non-zero exit, naming every fault at once. {#every-command-prints-exactly-one}
- `--help` is the manual: what the service is, what it needs (each variable by name, never its value), what to run first. Where the repo uses `agent-help`, help is written with it. {#help-is-the-manual-what-the}
- `--version` stamped by `build.rs`. {#version-stamped-by-build-rs}

## Nothing in the process

No database, no cache that is the truth, no file only this service
understands. A cache, if the task asks for one, is short-lived, bounded,
and losing it loses nothing.

## Words and decisions

New terms go in the glossary, decisions in the repo's decision records,
module and interface comments first, before the code they describe.
