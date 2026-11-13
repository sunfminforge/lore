# Passing Flux

Every function of every pure crate is checked by Flux refinement types
(skills/agent-cli.md).

## What to refine

- **Bounds**: never negative, never past a limit; **order**: this step
  only after that one, this step last.
- Flux proves what a call requires, not that the call happens: make the
  last step the only thing that can produce the return value.
- The proof starts at what the effect crate reports, not at the world:
  refine what crosses into the pure crate, and let the effect crate refuse
  what does not fit.
- Let the type that knows a limit make what depends on it: an index
  refined by a row's width, made only by the header that has that width,
  needs no bounds check where it is used; a conversion that takes a value
  refined by its kind cannot be written across kinds.
- State a struct's rule as its invariant (an id below the next id, a
  version that moves up by exactly one) and let only one method change it.
- What Flux cannot say, say with `proptest`.

## No exemptions

The gate requires `0 trusted; 0 ignored`. When a function does not verify:

1. Read the failing obligation: which refinement, at which call.
2. Strengthen the precondition the caller can prove, split the function so
   each part states what it needs, or change the type so the bad case
   cannot be built.
3. Never add `#[flux::trusted]`, `#[flux::ignore]` or their equivalents,
   never move the function out of the pure crate to escape the check, and
   never edit the gate to accept a non-zero count.

If a proof is out of reach within the task's budget, ask the foreman with
`guild task question`, naming the function and obligation.

## The toolchain

- Homebrew's `sunfmin/tap/flux-toolchain`, the newest the tap has, brings
  its own `fixpoint` and `z3`; pin and build none of them. Depend on the
  Flux crates at the revision `brew info sunfmin/tap/flux-toolchain` names.
  A pure crate opts in with `[package.metadata.flux]`.
- `verify.sh` refuses a missing or older toolchain, naming the `brew`
  command that fixes it; you install nothing yourself. A missing toolchain
  is a question to the foreman, not a reason to skip Flux.
- The flags (`-Ftimings -Ffixpoint-timeout=20 -Fcatch-bugs`) live in
  `verify.sh`, so a run by hand checks what the gate checks; the gate
  clears any inherited `FLUXFLAGS`. A query past twenty seconds fails
  naming its function: split or re-state it, never give it more time.
- Flux runs alone, before the tests. Flux attributes expand to nothing in
  a plain build: the gate runs nightly, a release builds on stable.
