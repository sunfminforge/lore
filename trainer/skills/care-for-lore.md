# Caring for lore as a whole

Roles go wrong in ways nobody points out: entries get too specific, a role
keeps growing or grows more complex than its work, and roles each look
after one narrow piece with no role seeing the level above them. These are
yours to find and fix, unasked.

## Taking lessons and findings

Take lessons, findings and signals as a batch, after Matt Pocock's retro
skill: read each one's evidence (its tasks, sessions and pull requests),
not only the line handed in, and strip it down to the way of working it
teaches; if nothing general is left, it goes into no role. Sort what is
left by where it holds best, the most severe first, and say them in that
order in the pull request:

- A mistake a check could catch (a pattern, a field, a path) becomes a detector, a tool or a check, not an entry asking to remember; first look whether one exists unwired or broken. Each is Rust, built the agent-cli way and named for `rust-cli-coder` to build, never a wrapped script: `guild forge check .` refuses one that embeds a script or names an interpreter. {#check-over-entry}
- A standard that a review can judge goes into the reviewing role, which reads a diff with room to spare, not into the role doing the work. {#standard-to-reviewer}
- Time spent finding something, or a call that cost too much, asks for a pointer where the instance looks first, or a tool request in forge. {#pointer-or-tool}
- What is left is an entry, the way a change goes. {#then-an-entry}
- `guild role draft` opens its pull with a stub body that may lack the `Closes` line, and pushes to origin, so pass `--remote` with Forgejo's lore URL where origin is a local copy. Write the body yourself: how each finding is answered, in the order above, and what you did not change and why; any `research-proposal` block, a fenced `toml research-proposal` with one `[[found]]` per thing adopted (name, source link, version or commit, licence, why); keep its `<!--guild:draft-->` line and a `Closes guild/tasks#<n>` line. {#pull-body}
- A draft answering a detector's finding: run that detector at the draft's head sha too and diff with main's output, until the finding is gone and none is new. {#rerun-the-detector}

## Which way a change goes

Before you add an entry, try in turn:

- **Rewrite** an existing entry so it holds the new case too. {#rewrite}
- **Merge** entries that say nearly the same. {#merge}
- **Simplify** what is used but more complex than it needs to be: a long procedure where one principle would do; a tool that only wraps another, merged or dropped; a detector another already covers, or whose findings nobody acts on. {#simplify}
- **Delete** what the evidence shows wrong, stale or never used, and what the task brief or a prohibition already holds: such a near-duplicate across roles is pruned, not lifted. {#delete}
- **Add**, only when none of these could hold it; the pull request says why nothing existing could. {#add-last}

A change that leaves a role shorter and as able is a good change. Before
asking for a new tool, ask whether an existing tool or a line in a skill
meets the need.

## The level above

When roles overlap, or the same failure or send-back reason shows up in
several roles, what is missing is a level above them. Lift what they share
into one place, and it lands on the approval that level needs:

- a shared skill each role names, where they do the same thing; {#shared-skill}
- a broader role, where they are one kind of work split too thin, merged through the role lifecycle; {#broader-role}
- the foreman's way of splitting, where tasks are cut so each lands on a role too narrow for it. {#foremans-split}

Split a role only on evidence from L3: its core over its cap with nothing
left to prune or move into skills, or its tasks falling into kinds that
fail or cost differently, not because a split looks tidier.

## Detectors for it

A detector is a forge tool, so prove it the way publish releases it, from
forge's root, before handing it in: `guild forge check .` reads `tool.toml`
as publish does (its usage one line, the tool's name then its arguments;
prose goes in the crate's README.md), and publish's own build,
`cargo build --release --locked --target <triple> -p <tool>`, makes the
binary to try: `guild detector try target/<triple>/release/<tool>` (a tool
name or an executable, not a crate dir). A crate's own `cargo test` passing
proves neither. Register it `--on goal-ended` and `--on daily` once it is
released from forge main: register refuses a tool not yet in the index.
Lore needs one for each of:

- **Memory outgrowing work**: a role's core and skills growing faster than the tasks it settles, over the same window. {#memory-outgrowing}
- **Specific entries**: an entry naming a repo, a path or an issue. {#specific-entries}
- **Near-duplicates**: entries that say nearly the same, within a role or across roles. {#near-duplicates}
- **One failure, several roles**: the same failure or send-back reason in the tasks of several roles. {#one-failure-many-roles}
- **Bouncing tasks**: a piece of work passed between narrow roles, task after task, before it lands. {#bouncing-tasks}

When you build a new one, run it over lore as it stands and propose what
it finds, each as its own role draft with its evidence.
