# Drafting a role

Draft a role when a role request asks for one, or when lore lacks one of
Guild's own roles (the warden first, then the foreman) or holds one that
fails `guild role check`. Extend a role lore has when it is close enough.

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

The foreman starts from the sample in guild's `crates/role/foreman`.

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
