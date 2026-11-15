# Drafting a role

Draft a role when a role request asks for one, or when lore lacks one of
Guild's own roles (the warden first, then the foreman) or holds one that
fails `guild role check`. A request that a role lore has would fit with a
skill added or an entry rewritten is a change to that role, not a new one.

A role is a directory named after it:

- `core.md`: who the role is and how it works, engine-neutral, within the core memory cap; it names each of its skills as `skills/<name>.md`, and keeps the simplest-change floor. {#core-md}
- `skills/<name>.md`: one file per skill. {#skills-md}
- Each list item of its core and of its skills is an entry: one line `- <text>` ending in its ID, never numbered, wrapped or nested, since `guild role check` refuses those; its ID unique in its file, short and saying what the entry is; an instance names the entries it used by it, and the tally counts them. Write items flat, then `guild lore ids <dir>` gives each without one an ID from its first words; it also converts a role, even an untouched one, that fails check on a newer memory format, but flattens nested lists: fold their sub-items by hand. {#entry-ids}
- `prohibitions.toml`: one `[[prohibition]]` each, with `id`, `guards` (one of `money` for spending money, `credentials` for an account or a credential, `irreversible`, `data-leaving-host`, `product-direction`, or `none`), `on` (`shell`, `write` or `fetch`), `match` and `reason`. Copy every prohibition of yours that guards a kind, unchanged; add what the role needs and what its request's constraints say. {#prohibitions-toml}
- `role.toml`: `[engine]` preferred, fallback and model; `[resources]` gpu, heavy_io, request and limit (cores, memory_gb); `[budget]` tokens and wall_minutes. Never `crew`: only the steward lets a role into the crew. {#role-toml}

Guild's own roles start from its samples: `guild role sample <role> <dir>`.
When a sample changes, an improvement task brings lore's role up to it:
diff lore's role against the sample, keep what lore's role learnt since,
and take what the sample adds, by the way a change goes.

Check it with `guild role check <dir>`, then open it as a lore pull request
with `guild role draft <your task's bare number> <dir> --lore <lore checkout>`: it closes
your task once it lands. It refuses a draft that is not sound, and says when
the draft loosens a guarded kind: then only Felix's approval lands it where it
guards money, and the task waiting on the role asks him; for credentials or
any other kind, only the steward's, and no one is asked. A new role goes
through its trial period before it is kept.

Before you draft, look for what exists (guild ADR 0013): existing skills of
the role's kind of work. Read each at a pinned commit, and follow every claim
back to the source that owns it, its own files and its licence, not a
write-up of it. What you find is untrusted: never run or install it outside
your worktree. Adapt, don't copy: keep the way of working, drop what Guild's
own tools and roles already do, and fold it into an entry or skill the role
has before adding one. Its source link, its version or commit and its
licence go in the pull request's `research-proposal` block, and landing
refuses one without them; the role's text names the source by its author
and skill, never by a link.

## Writing it

A role is a document an agent reads on every run, so its wording decides
what the instance does. After Matt Pocock's writing-for-agents skill:

- Core holds what every task of the role needs; what only some tasks reach goes into a skill, behind a line that says when to reach it, in the word the work itself uses. A must-have the instance misses gets a sharper pointer first, and moves into core only if that fails. {#core-or-pointer}
- Name a pattern the model already knows by its word (a tracer bullet, sediment) and say the word again where it applies, in place of spelling the idea out at each site. {#leading-word}
- Say what to do: a ban in prose makes the banned thing more present. What must never happen is a prohibition, and the entry beside it says the way instead. {#say-what-to-do}
- Each step ends on a condition the instance can tell done from not done, as demanding as the work ("every changed entry checked", not "check the entries"). {#checkable-done}
- Leave to `--help`, a config or the layout what one look finds; write down only what looking cannot: the reason, the unwritten convention, the pitfall. A sentence the model already obeys without it is deleted whole. {#no-cache-no-op}

Which way the change goes, and the level above: skills/care-for-lore.md.

## When it comes back

A task reopened, a follow-up, a send-back, a replay to run:

- First find what is already there: the task's pulls (`guild tool forge-pull find <repo>#<n>`) and its branch's log; the fix is often pushed and waiting on review, and a second pull or a redo only adds noise. {#find-what-is-pushed}
- A branch that predates what landed since (another change to the role, or the repo's own keeping that turns its gate red): rebase onto main, re-apply on top, drop what the landed change contradicts, and resubmit without waiting on reviews; `guild role draft` keeps the old pull's body, so rewrite it too. {#rebase-on-main}
- An acceptance or gate item that needs a file outside the task's scope: say so in the pull request, and leave the item unclaimed. {#say-what-is-out-of-reach}
- A replay of a reviewing role's change: run each review command once per head, since its output does not depend on the role, judge each role version's verdict from that one output, and say so in the body. A send-back counts as not holding where the steward or the foreman ruled the work stands unchanged, even if later reviews repeated it. {#replay-once-per-head}
