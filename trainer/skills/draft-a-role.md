# Drafting a role

Draft a role when a role request asks for one, or when lore lacks one of
Guild's own roles (the warden first, then the foreman) or holds one that
fails `guild role check`. A request that a role lore has would fit with a
skill added or an entry rewritten is a change to that role, not a new one.

A role is a directory named after it:

- `core.md`: who the role is and how it works, engine-neutral, within the core memory cap; it names each of its skills as `skills/<name>.md`, and keeps the simplest-change floor and, for a working role, the talk floor word for word. {#core-md}
- `skills/<name>.md`: one file per skill, named from core, one level deep: a skill sends to no further skill for what its work needs. {#skills-md}
- Each list item of its core and of its skills is an entry: one line `- <text>` ending in its ID, never numbered, wrapped or nested, since `guild role check` refuses those; its ID unique in its file, short and saying what the entry is; an instance names the entries it used by it, and the tally counts them. Write items flat, then `guild lore ids <dir>` gives each without one an ID from its first words; it also converts a role, even an untouched one, that fails check on a newer memory format, but flattens nested lists: fold their sub-items by hand. A change to a role lore holds is entries only, added, updated or deleted by ID with `guild lore curate`: `guild lore check . --base <main>` refuses a changed heading or prose line, so a pointer or a section goes inside an entry; a role converted by `lore ids` shows in its pull request that its text is unchanged once IDs and wrapping are stripped. {#entry-ids}
- `prohibitions.toml`: one `[[prohibition]]` each, with `id`, `guards` (one of `money` for spending money, `credentials` for an account or a credential, `irreversible`, `data-leaving-host`, `product-direction`, or `none`), `on` (`shell`, `write` or `fetch`), `match` and `reason`. Copy every prohibition of yours that guards a kind, unchanged; add what the role needs and what its request's constraints say. {#prohibitions-toml}
- `role.toml`: `[engine]` preferred, fallback and model; `[resources]` gpu, heavy_io, request and limit (cores, memory_gb); `[budget]` tokens and wall_minutes. Never `crew`: only the steward lets a role into the crew. {#role-toml}
- Research comes first, for every draft and every re-research of a role (guild ADR 0013): how practitioners do its kind of work well, not its request alone; `guild role draft` takes it with `--research <file>` and refuses a new role's draft without it. {#research-first}
- Where to look: the platform's own guides, well-regarded projects of that kind and their contributor guides, and published agent skills of that kind. {#research-sources}
- What to find: its workflow, its toolchain's setup, testing, project layout and common pitfalls, each with its source link and the date read; where the work runs a toolchain, how practitioners set it up headless and declarative, its project files generated from a text source and never hand-edited, written as a shared skill every role on that toolchain names. {#research-covers}
- The best tools for it: what each is for, why it beats the alternatives found, and how to use it headless; it is installed by forge or as an external tool when a role needs it, never preinstalled, and an outside service is researched down the rungs `guild forge ladder` prints, stopping at the first that serves. {#research-tools}
- Each practice found goes in as a general entry or says why not, in the `role-research` block `--research` takes, a fenced block opened as toml role-research: `searched = [...]`, one `[[practice]]` per practice (practice, why, source, date, and `entry = "<id>"` or `not_taken = "<why>"`), one `[[tool]]` per tool (name, for, why, how, install `forge` or `external`, source, date). {#research-practices}
- A skill adopted from outside goes in the pull's `research-proposal` block, TOML, one `[[found]]` per thing adopted: name, source (its https link), version or commit, licence and why, and service and rung for a tool serving an outside service; no other field is read. Write it in the role's own objects (its records, its proofs, its outputs), naming the shared idea by its usual term: one another role took too, in the same words, is a near-duplicate. {#research-proposal}
- `guild role draft` takes the bare task number and opens one pull per role directory, with a stub body: edit the body then to say how the change answers each finding, in the order taken, and add any `research-proposal` block. {#pull-body}
- Detectors read Forgejo, not your worktree: to see a draft's effect, run them with `GUILD_LORE_BRANCH` set to the draft's branch, URL-encoded, and diff their sorted findings against main's run; only what the draft changed should differ. {#detector-on-draft}

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

- Core holds what every task of the role needs; what only some tasks reach goes into a skill, behind a line that says when to reach it, in the word the work itself uses. A must-have the instance misses gets a pointer from the core entry every run reads, carrying the check itself, and moves into core only if that fails; a rule a review applies on every run of its kind sits in that core entry, not behind a pointer. {#core-or-pointer}
- Write for every model the role may run on, as briefly as the strongest needs and as plainly as the weakest; match how exact a step is to how fragile it is: one command for a step that breaks, a principle where several ways serve. {#degrees-of-freedom}
- Name a pattern the model already knows by its word (a tracer bullet, sediment) and say the word again where it applies, in place of spelling the idea out at each site. {#leading-word}
- Say what to do: a ban in prose makes the banned thing more present. What must never happen is a prohibition, and the entry beside it says the way instead. {#say-what-to-do}
- Each step ends on a condition the instance can tell done from not done, as demanding as the work ("every changed entry checked", not "check the entries"). {#checkable-done}
- Leave to `--help`, a config or the layout what one look finds; write down only what looking cannot: the reason, the unwritten convention, the pitfall. A sentence the model already obeys without it is deleted whole. {#no-cache-no-op}

Which way the change goes, and the level above: skills/care-for-lore.md.

## When it comes back

A task reopened, a follow-up, a send-back, a replay to run:

- First find what is already there: the task's pulls (`guild tool forge-pull find <repo>#<n>`) and its branch's log; the fix is often pushed and waiting on review, and a second pull or a redo only adds noise. {#find-what-is-pushed}
- A branch that predates what landed since (another change to the role, or the repo's own keeping that turns its gate red): start again from origin/main, since an old base silently reverts what landed, guarded prohibitions too; re-apply on top, drop what the landed change contradicts, and re-run `guild role draft`, which updates the same pull, without waiting on reviews; it keeps the old pull's body, so rewrite it too. {#rebase-on-main}
- An acceptance or gate item that needs a file outside the task's scope: say so in the pull request, and leave the item unclaimed. {#say-what-is-out-of-reach}
- A replay of a reviewing role's change: run each review command once per head, since its output does not depend on the role, judge each role version's verdict from that one output, and say so in the body. A send-back counts as not holding where the steward or the foreman ruled the work stands unchanged, even if later reviews repeated it. {#replay-once-per-head}
- A replay is an evaluation against a baseline: diff the rendered instance context of both sides first (a change only to role.toml differs only by placement); freeze each run's lore refs at the review's time and make that frozen guild the only one the session can reach, since PATH shims do not sandbox it; hand it the pull body the review read, taken from its session transcript; run the case that splits the sides four times or more each, since two runs are within variance; check in the transcript that the changed entry was read before judging its wording; and where both sides fail a check the old head passed, call it tool drift, not a tie. A level score means the deciding case is missing: read its history for what its reviewer could not see. {#replay-as-eval}
- A replay task's pull carries its replay block and no closing line, since landing waits on the replay; in prose of a body Guild parses, write "replay block", never a fence opener. {#replay-pull-body}
