# Testing on recorded fixtures

The loop and the bug hunt here are adapted from the tdd and
diagnosing-bugs skills of Matt Pocock's skills collection, at commit
5863806 (MIT); the pull request that adopted them links the source. The
repo's `gate.sh` and decision records outrank this page.

## Fixtures

- A fixture is a real answer of the API, recorded once, saved as the JSON the API sent, under `fixtures/<api>/<what>.json`, with a line in a `README` beside it saying the request that produced it and when. {#a-fixture-is-a-real-answer-of}
- Before committing one, strip it: no token, no cookie, no header, no private address or e-mail beyond what the test needs. Replace them with plainly fake values. A fixture is read by anyone who reads the repo. {#before-committing-one-strip-it}
- Record the cases the code must handle, not only the happy one: empty lists, paging, a missing field, an error body. When the API changes, record again; never hand-edit a fixture into a shape the API does not send. {#record-the-cases-the-code-must}
- Tests never call a live API; the gate runs without a network and without a token. {#tests-never-call-a-live-api-the}

## What each layer proves

- **Pure crates**: parse each fixture into the pure types, compute, and assert the result. Plain unit tests for the cases, `proptest` for what must hold for every input (ordering, totals, no panic on any page). {#pure-crates-parse-each-fixture}
- **Rendering**: render each view model and assert what a reader needs is there: the values, the links, an empty state, an error state. Snapshot tests (`insta`) where the whole page is the point, reviewed when they change, never accepted blind. {#rendering-render-each-view-model}
- **Effect crate**: run the real binary against a stand-in server started in the test (axum on a random loopback port) that serves the fixtures, records what it was asked and can fail on demand. Assert the requests were `GET`, carried the token only in the header, and that `check`'s JSON and exit code are right when a source is up, down or refuses the token. {#effect-crate-run-the-real-binary}
- **The token**: one test sets a known token, runs `check`, `serve` and a failing read, and asserts the token appears in no stdout, stderr, page or log. {#the-token-one-test-sets-a-known}

The stand-in and the real code share the same paths and types, so a
change to one fails the other's test.

## Test first

- A test goes through what a caller uses: a pure crate's public functions, a rendered view, the binary's JSON and exit code. One that peeks at private state, or breaks when a refactor kept every answer the same, has gone past the interface. {#through-what-a-caller-uses}
- What a test expects is read off the fixture by hand (this page's count, that item's title) or taken from the API's documentation, never recomputed in the test the way the crate computes it. {#expected-read-off-the-fixture}
- The stand-in server and fixtures are the only fakes, since the API is where the world begins; the pure crates are called as they are, never mocked. {#only-the-api-is-faked}
- Cycle by one behaviour: record or pick the fixture, write one test on it, run it and read it fail on its assertion (not on a compile error or a missing file), then write only what passes it. Writing every test before any code tests an imagined API. {#cycle-by-one-behaviour}
- Once green, refactor with the tests untouched; a test you must edit for a change that kept the answers was testing past the interface. Run the gate after. {#refactor-with-tests-untouched}

## Chasing a bug

- Before any theory, get one command, run already, that fails on the reported symptom and on nothing else, every run, in seconds: usually the real answer that showed it, recorded and stripped as a fixture, fed to the pure crate or served by the stand-in server. {#fails-on-the-reported-symptom}
- When it only sometimes fails, pin what varies (the clock passed in, the order of pages served, concurrent requests to the stand-in) and repeat it until it fails often. If no such command can be built, ask the foreman with `guild task question`, saying what you tried, rather than guessing a fix. {#pin-what-varies}
- Shrink that fixture or request one element at a time, rerunning after each, until removing anything left makes it pass. {#shrink-the-fixture}
- Write down three to five likely causes with what each predicts, and try them in order, changing one thing at a time. Probes go to stderr under one tag, never print the token, and are all removed by that tag before you hand in. {#write-down-likely-causes}
- The shrunk case becomes the regression test at the layer where the bug lives, failing before the fix and passing after; then the first command is run again. If no layer reaches it, the pull request says so rather than testing something nearby, and it names the cause found. {#shrunk-case-is-the-regression}
