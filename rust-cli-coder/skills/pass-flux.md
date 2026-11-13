# Passing Flux

Every function of every pure crate is checked by Flux refinement types.

## What to refine

- **Bounds**: never negative, never past a limit.
- **Order**: this step only after that one; this step last.
- Flux proves what a call requires, not that the call happens: make the
  last step the only thing that can produce the return value.
- The proof starts at what the effect crate reports, not at the world:
  refine what crosses into the pure crate, and let the effect crate refuse
  what does not fit.
- What Flux cannot say, say with `proptest`.

## No exemptions

The gate reads Flux's summary and requires `0 trusted; 0 ignored`. When a
function does not verify:

1. Read the failing obligation: which refinement, at which call.
2. Strengthen the precondition the caller can prove, split the function so
   each part states what it needs, or change the type so the bad case
   cannot be built.
3. Never add `#[flux::trusted]`, `#[flux::ignore]` or their equivalents,
   never move the function out of the pure crate to escape the check, and
   never edit the gate to accept a non-zero count.

If a proof is out of reach within the task's budget, stop and ask the
foreman with `guild task question`, saying which function and obligation;
do not hand in a gate that only passes by an exemption.

## The toolchain

- Check with Homebrew's `sunfmin/tap/flux-toolchain`, the newest the tap
  has; it brings its own `fixpoint` and `z3`, so pin and build none of
  them. Depend on the Flux crates at the revision
  `brew info sunfmin/tap/flux-toolchain` names.
- `verify.sh` refuses a missing or older toolchain and names the `brew`
  command that fixes it; you install nothing yourself. A missing toolchain
  is a question to the foreman, not a reason to skip Flux.
- Flux attributes expand to nothing in a plain build: the gate runs
  nightly, a release builds on stable.
- Flux runs alone, before the tests, with
  `FLUXFLAGS="-Ftimings -Ffixpoint-timeout=20 -Fcatch-bugs"`: a solver
  query past twenty seconds fails naming its function. A slow function is
  split or re-stated until it is fast, not given more time.
