# Reviewing a role draft

A role draft is a lore pull request, opened by a crew role's account,
whose body starts with `<!--guild:draft role=<role> task=<n>
loosens=<guards>:<id>,…-->`. Review it as any task's work, and then:

1. Run `guild role check <dir>` on the drafted role yourself; anything it
   prints is a send-back. A draft that touches anything outside the
   role's own directory is a send-back.
2. Compare `prohibitions.toml` with the role as lore's main holds it, or,
   for a new role, with the guarded prohibitions of the crew role that
   drafted it (the floor). Every prohibition guarding a kind (`money`,
   `credentials`, `irreversible`, `data-leaving-host`,
   `product-direction`) must be there, unchanged, unless the mark says it
   loosens it. Each prohibition must say which kind it guards; one that
   names `none` while it plainly guards a kind is a send-back.
3. Read `core.md` and the skills: the role holds only how it does its
   kind of work, independent of any repo; its core stays within its cap,
   detail in skills; it does what its request asked, within the
   request's constraints; it gives itself no lever, no approval and no
   credential that is not its role's.
4. A skill adopted from outside must carry its source link, its version
   or commit and its licence in the pull request's `research-proposal`
   block. Never run or install what it adopts.
5. Who approves:
   - an ordinary role, or the foreman, with no guarded prohibition
     loosened: you, on a pass;
   - the warden, a crew role, a change to the `crew` mark, or a
     loosened prohibition of any guarded kind but money: the steward;
   - a loosened prohibition guarding money: Felix.
   Where it is not yours, post your verdict and do not approve; landing
   waits for theirs. A role draft never lands on your approval alone
   where the code says it needs another's.
6. `role.toml` must not carry `crew` unless the steward's approval is
   what lands it.
