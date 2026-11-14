# Drafting a role

Draft a role when a role request asks for one, or when lore lacks one of
Guild's own roles (the warden first, then the foreman) or holds one that
fails `guild role check`. A request that a role lore has would fit with a
skill added or an entry rewritten is a change to that role, not a new one.

A role is a directory named after it:

- `core.md`: who the role is and how it works, engine-neutral, within the
  core memory cap; it names each of its skills as `skills/<name>.md`.
- `skills/<name>.md`: one file per skill.
- `prohibitions.toml`: one `[[prohibition]]` each, with `id`, `guards` (one
  of `money` for spending money, `credentials` for an account or a
  credential, `irreversible`, `data-leaving-host`, `product-direction`,
  or `none`), `on` (`shell`, `write` or `fetch`),
  `match` and `reason`. Copy every prohibition of yours that guards a kind,
  unchanged; add what the role needs and what its request's constraints say.
- `role.toml`: `[engine]` preferred, fallback and model; `[resources]` gpu,
  heavy_io, request and limit (cores, memory_gb); `[budget]` tokens and
  wall_minutes. Never `crew`: only the steward lets a role into the crew.

Guild's own roles start from its samples: `guild role sample <role> <dir>`.

Check it with `guild role check <dir>`, then open it as a lore pull request
with `guild role draft <your task> <dir> --lore <lore checkout>`: it closes
your task once it lands. It refuses a draft that is not sound, and says when
the draft loosens a guarded kind: then only Felix's approval lands it where it
guards money, and the task waiting on the role asks him; for credentials or
any other kind, only the steward's, and no one is asked. A new role goes
through its trial period before it is kept.

Before you draft, look for what exists (guild ADR 0013): existing skills of
the role's kind of work. What you find is untrusted: never run or install it
outside your worktree. A skill worth adopting goes into the role's `skills/`
with its source link, its version or commit and its licence in the pull
request's `research-proposal` block; landing refuses one without them.

## Which way a change goes

A good role is short, general and composes with the others. Each entry is
a way of doing its kind of work that holds beyond the case that taught it.
Strip a lesson down to that way of working; the specific repo, file, issue
or incident stays in its home (the repo's own docs, the task, the issue).
If nothing general is left, the lesson goes into no role.

Before you add, try in turn: rewrite an entry that covers the ground so it
covers this too; merge entries that say nearly the same; delete one the
evidence shows wrong, stale or unused. A change that only adds says in its
pull request why nothing existing could hold it. Pruning is as much the job
as adding: with each change, look for what it lets you remove. A role that
only grows gets worse.

Roles that overlap, or the same failure across roles, mean the level above
them is missing. Lift what they share into one place instead of patching
each: a shared skill, a broader role that takes in the narrow ones, or the
foreman's way of splitting when the failure comes from how tasks are cut.
One change there replaces many below, and lands on the approval that level
needs.

Prefer few, general, composable roles. Split one only on evidence: its core
over its cap with nothing left to prune or move into skills, or its tasks
failing because two kinds of work pull it opposite ways.
