# Passing Flux

Every function of every pure crate is checked by Flux refinement types
(skills/agent-cli.md).

## What to refine

- **Bounds**: never negative, never past a limit; **order**: this step only after that one, this step last. {#bounds-and-order}
- Flux proves what a call requires, not that the call happens: make the last step the only thing that can produce the return value. {#last-step-only-way}
- The proof starts at what the effect crate reports, not at the world: refine what crosses into the pure crate, and let the effect crate refuse what does not fit. {#proof-starts-at-effect-crate}
- Let the type that knows a limit make what depends on it: an index refined by a row's width, made only by the header that has that width, needs no bounds check where it is used; a conversion that takes a value refined by its kind cannot be written across kinds. {#limit-type-makes-dependents}
- State a struct's rule as its invariant (an id below the next id, a version that moves up by exactly one) and let only one method change it. {#struct-invariant}
- What Flux cannot say, say with `proptest`, its strategy started at the edges random input rarely reaches (a bound, a DST transition found from the zone's offsets), or a broken rule stays green. {#proptest-for-the-rest}
- Write the refinement before the body that meets it and run Flux to see it refuse the code as it stands: red first, as in skills/test-the-binary.md. A refinement never seen to fail proves nothing new. {#refinement-before-body}

## No exemptions

The gate requires `0 trusted; 0 ignored`. When a function does not verify:

- Read the failing obligation: which refinement, at which call. {#read-the-obligation}
- Strengthen the precondition the caller can prove, split the function so each part states what it needs, or change the type so the bad case cannot be built. {#fix-the-code-not-the-check}
- Flux cannot see through `RangeInclusive::contains`, and clippy refuses the comparison written out: state a bound as a range pattern (`let 1..=12 = m else`, `Some(k @ 1..=5)`) or a helper refined `-> bool[lo <= x && x <= hi]`, which both accept. {#bounds-as-range-patterns}
- Under strict overflow checking, prefer `saturating_sub`, `saturating_add` and `split_once` to index arithmetic in loop and string code (a `-= 1` under `while x > 0` with a `?` in the body is refused): they pass with no refinement work and loosen nothing. {#saturating-over-index-arithmetic}
- Where Flux crashes or cannot read a construct (an iterator chain with closures, a slice pattern, a macro that unwraps such as `json!`, a derive on a struct with an invariant), write it plainly: a loop, `Map::insert`, a hand-written impl. {#rewrite-what-flux-cannot-read}
- Never add `#[flux::trusted]`, `#[flux::ignore]` or their equivalents, never move the function out of the pure crate to escape the check, and never edit the gate to accept a non-zero count. {#never-trust-or-ignore}

If a proof is out of reach within the task's budget, ask the foreman with
`guild task question`, naming the function and obligation.

## The toolchain

- Homebrew's Flux toolchain formula, the one `verify.sh` names, at the newest its tap has, brings its own `fixpoint` and `z3`; pin and build none of them. Depend on the Flux crates at the revision `brew info` names for that formula. A pure crate opts in with `[package.metadata.flux]`. {#flux-toolchain}
- `verify.sh` refuses a missing or older toolchain, naming the `brew` command that fixes it; you install nothing yourself. A missing toolchain is a question to the foreman, not a reason to skip Flux. {#verify-refuses-old-toolchain}
- The flags (`-Ftimings -Ffixpoint-timeout=20 -Fcatch-bugs`) live in `verify.sh`, so a run by hand checks what the gate checks; the gate clears any inherited `FLUXFLAGS`. A query past twenty seconds fails naming its function: split or re-state it, never give it more time. {#flags-in-verify-sh}
- Flux runs alone, before the tests. Flux attributes expand to nothing in a plain build: the gate runs nightly, a release builds on stable. {#flux-runs-alone}
