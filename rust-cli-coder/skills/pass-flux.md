# Passing Flux

Every function of every pure crate is checked by Flux refinement types
(skills/agent-cli.md).

## What to refine

- **Bounds**: never negative, never past a limit; **order**: this step only after that one, this step last. {#bounds-never-negative-never-past}
- Flux proves what a call requires, not that the call happens: make the last step the only thing that can produce the return value. {#flux-proves-what-a-call-requires}
- The proof starts at what the effect crate reports, not at the world: refine what crosses into the pure crate, and let the effect crate refuse what does not fit. {#the-proof-starts-at-what-the}
- Let the type that knows a limit make what depends on it: an index refined by a row's width, made only by the header that has that width, needs no bounds check where it is used; a conversion that takes a value refined by its kind cannot be written across kinds. {#let-the-type-that-knows-a-limit}
- State a struct's rule as its invariant (an id below the next id, a version that moves up by exactly one) and let only one method change it. {#state-a-struct-s-rule-as-its}
- What Flux cannot say, say with `proptest`. {#what-flux-cannot-say-say-with}
- Write the refinement before the body that meets it and run Flux to see it refuse the code as it stands: red first, as in skills/test-the-binary.md. A refinement never seen to fail proves nothing new. {#refinement-before-body}

## No exemptions

The gate requires `0 trusted; 0 ignored`. When a function does not verify:

- Read the failing obligation: which refinement, at which call. {#read-the-failing-obligation}
- Strengthen the precondition the caller can prove, split the function so each part states what it needs, or change the type so the bad case cannot be built. {#strengthen-the-precondition-the}
- Never add `#[flux::trusted]`, `#[flux::ignore]` or their equivalents, never move the function out of the pure crate to escape the check, and never edit the gate to accept a non-zero count. {#never-add-flux-trusted-flux}

If a proof is out of reach within the task's budget, ask the foreman with
`guild task question`, naming the function and obligation.

## The toolchain

- Homebrew's Flux toolchain formula, the one `verify.sh` names, at the newest its tap has, brings its own `fixpoint` and `z3`; pin and build none of them. Depend on the Flux crates at the revision `brew info` names for that formula. A pure crate opts in with `[package.metadata.flux]`. {#homebrew-s-sunfmin-tap-flux}
- `verify.sh` refuses a missing or older toolchain, naming the `brew` command that fixes it; you install nothing yourself. A missing toolchain is a question to the foreman, not a reason to skip Flux. {#verify-sh-refuses-a-missing-or}
- The flags (`-Ftimings -Ffixpoint-timeout=20 -Fcatch-bugs`) live in `verify.sh`, so a run by hand checks what the gate checks; the gate clears any inherited `FLUXFLAGS`. A query past twenty seconds fails naming its function: split or re-state it, never give it more time. {#the-flags-ftimings-ffixpoint}
- Flux runs alone, before the tests. Flux attributes expand to nothing in a plain build: the gate runs nightly, a release builds on stable. {#flux-runs-alone-before-the-tests}
