# Drafting a role

Draft a role when a role request asks for one, or when lore lacks one of
Guild's own roles (the warden first, then the foreman) or holds one that
fails `guild role check`. A request that a role lore has would fit with a
skill added or an entry rewritten is a change to that role, not a new one.

A role is a directory named after it:

- `core.md`: who the role is and how it works, engine-neutral, within the core memory cap; it names each of its skills as `skills/<name>.md`, and keeps the simplest-change floor. {#core-md}
- `skills/<name>.md`: one file per skill. {#skills-md}
- Each list item of its core and of its skills is an entry: one line ending in its ID, `{#<id>}`, unique in its file, short and saying what the entry is; an instance names the entries it used by it, and the tally counts them. `guild lore ids <dir>` gives each item without one an ID from its first words. {#entry-ids}
- `prohibitions.toml`: one `[[prohibition]]` each, with `id`, `guards` (one of `money` for spending money, `credentials` for an account or a credential, `irreversible`, `data-leaving-host`, `product-direction`, or `none`), `on` (`shell`, `write` or `fetch`), `match` and `reason`. Copy every prohibition of yours that guards a kind, unchanged; add what the role needs and what its request's constraints say. {#prohibitions-toml}
- `role.toml`: `[engine]` preferred, fallback and model; `[resources]` gpu, heavy_io, request and limit (cores, memory_gb); `[budget]` tokens and wall_minutes. Never `crew`: only the steward lets a role into the crew. {#role-toml}

Guild's own roles start from its samples: `guild role sample <role> <dir>`.
When a sample changes, an improvement task brings lore's role up to it:
diff lore's role against the sample, keep what lore's role learnt since,
and take what the sample adds, by the way a change goes.

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

Which way the change goes, and the level above: skills/care-for-lore.md.
