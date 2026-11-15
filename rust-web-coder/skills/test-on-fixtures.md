# Testing on recorded fixtures

A fixture is the test's whole world, so the cycle and the bug hunt below
both start from one; they follow the tdd and diagnosing-bugs skills of
Matt Pocock (MIT), whose commit the adopting pull request pins.

## Fixtures

- A fixture is a real answer of the API, recorded once, saved as the JSON the API sent, under `fixtures/<api>/<what>.json`, with a line in a `README` beside it saying the request that produced it and when. {#fixture-is-recorded-answer}
- Before committing one, strip it: no token, no cookie, no header, no private address or e-mail beyond what the test needs. Replace them with plainly fake values. A fixture is read by anyone who reads the repo. {#strip-fixtures}
- Record the cases the code must handle, not only the happy one: empty lists, paging, a missing field, an error body. When the API changes, record again; never hand-edit a fixture into a shape the API does not send. {#record-edge-cases}
- Tests never call a live API; the gate runs without a network and without a token. {#no-live-api}

## What each layer proves

- **Pure crates**: parse each fixture into the pure types, compute, and assert the result. Plain unit tests for the cases, `proptest` for what must hold for every input (ordering, totals, no panic on any page). {#test-pure-crates}
- **Rendering**: render each view model and assert what a reader needs is there: the values, the links, an empty state, an error state. Snapshot tests (`insta`) where the whole page is the point, reviewed when they change, never accepted blind. {#test-rendering}
- **Effect crate**: run the real binary against a stand-in server started in the test (axum on a random loopback port) that serves the fixtures, records what it was asked and can fail on demand. Assert the requests were `GET`, carried the token only in the header, and that `check`'s JSON and exit code are right when a source is up, down or refuses the token. {#test-effect-crate}
- **The token**: one test sets a known token, runs `check`, `serve` and a failing read, and asserts the token appears in no stdout, stderr, page or log. {#test-token-hidden}

The stand-in and the real code share the same paths and types, so a
change to one fails the other's test.

## Red, green, refactor on a fixture

- Cycle by one behaviour: pick or record the fixture that shows it, write one test on it, and read it fail on its assertion (not on a compile error or a missing file) before writing only what passes it. Every test written before any code tests an imagined API. {#cycle-by-one-behaviour}
- A test goes through what a caller uses: a pure crate's public functions, a rendered view, the binary's JSON and exit code. Refactor once green, with the tests untouched; a test you must edit when every answer stayed the same was testing past the interface. {#through-what-a-caller-uses}
- What a test expects is read off the fixture by hand (this page's count, that item's title) or from the API's documentation; a total summed in the test the way the crate sums it agrees by construction. {#expected-read-off-the-fixture}
- The stand-in server and the fixtures are the only fakes, since the API is where the world begins; the pure crates are called as they are, never mocked. {#only-the-api-is-faked}

## Chasing a bug

- Before any theory, capture the real answer that showed it, strip it as a fixture, and run a test that feeds it to the pure crate, or serves it from the stand-in, and fails on the symptom reported and on nothing else, every run, in seconds. {#fails-on-the-reported-symptom}
- When it fails only sometimes, pin what varies (the clock passed in, the order pages are served, concurrent requests to the stand-in) and repeat until it fails often. If no such test can be built, ask the foreman with `guild task question`, saying what you tried, rather than guess a fix. {#pin-what-varies}
- Shrink that fixture one element at a time, rerunning after each, until removing anything left makes it pass. {#shrink-the-fixture}
- List the causes the shrunk fixture leaves, each with the change that would prove it, and try one change at a time; a probe logs to stderr under one tag, and the token never appears in it. {#write-down-likely-causes}
- That fixture's test stays as the regression test at the layer where the bug lives (pure crate, view, or stand-in run), seen failing before the fix; then rerun the first test. A bug no layer reaches is said in the pull request. Finished when it passes, no probe tag is left, and the pull request names the cause. {#shrunk-case-is-the-regression}
