# Testing on recorded fixtures

## Fixtures

- A fixture is a real answer of the API, recorded once, saved as the JSON
  the API sent, under `fixtures/<api>/<what>.json`, with a line in a
  `README` beside it saying the request that produced it and when.
- Before committing one, strip it: no token, no cookie, no header, no
  private address or e-mail beyond what the test needs. Replace them with
  plainly fake values. A fixture is read by anyone who reads the repo.
- Record the cases the code must handle, not only the happy one: empty
  lists, paging, a missing field, an error body. When the API changes,
  record again; never hand-edit a fixture into a shape the API does not
  send.
- Tests never call a live API; the gate runs without a network and
  without a token.

## What each layer proves

- **Pure crates**: parse each fixture into the pure types, compute, and
  assert the result. Plain unit tests for the cases, `proptest` for what
  must hold for every input (ordering, totals, no panic on any page).
- **Rendering**: render each view model and assert what a reader needs is
  there: the values, the links, an empty state, an error state. Snapshot
  tests (`insta`) where the whole page is the point, reviewed when they
  change, never accepted blind.
- **Effect crate**: run the real binary against a stand-in server started
  in the test (axum on a random loopback port) that serves the fixtures,
  records what it was asked and can fail on demand. Assert the requests
  were `GET`, carried the token only in the header, and that `check`'s
  JSON and exit code are right when a source is up, down or refuses the
  token.
- **The token**: one test sets a known token, runs `check`, `serve` and a
  failing read, and asserts the token appears in no stdout, stderr, page
  or log.

The stand-in and the real code share the same paths and types, so a
change to one fails the other's test.
