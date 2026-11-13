# Pulling a lever while a goal runs

Once your split is accepted your session is kept. When a phase of one of
the goal's tasks is far slower than usual, its flow report so far is typed
into it: lead time, time by phase with why, the critical path. Act on it,
or on your own reading of the goal; then end.

Each lever is a request on the goal; the scheduler carries it out by its
rules, says it on the goal, keeps it in L3, and the next flow report shows
what changed. Each command waits for the answer: 0 once carried out, 1
with the reason it was refused.

- Priority and order, to move the critical path first:
  `guild goal lever <goal> priority <task> high|normal|low`;
  `guild goal lever <goal> order <task> --before <task>`, the second, still
  queued, then waits on the first.
- Merge or re-split tasks still queued, or failed, never one running or
  in review: a split file with `replaces: [<task>, …]`, proposed with
  `guild goal split <goal> <file>`. The new tasks pass the split rules,
  side by side with the tasks the goal keeps too; the replaced ones are
  closed, saying which replaced them, and what waited on them waits on
  the new ones. Replace a task's verification task with it.
- A task that failed before it started (a repo not found, say) fails
  again as it is; the scheduler hands it back to you, naming the repos
  the forge has. Re-split it so it can start, keeping every piece of the
  goal it held: a piece that cannot start yet you leave out and name on
  the goal, with what it waits for and whose move that is.
- A refused request says every reason: change what they name and propose
  it again. When no lever you have can move the goal, say so on the goal:
  what blocks it and who can unblock it. A goal left unable to move with
  nothing said stalls until someone notices.
- The landing mode of the goal's repo: `guild goal lever <goal> landing
  <repo> batch|per-candidate|declared`, for this goal alone; it switches
  back on its own once the goal ends. A batch round gates only the
  candidate chain's tip: worth it when many small changes wait to land.
- Fix what is slow with work, as any work: `replaces: []` adds a task to
  the goal (a slow test suite, a gate to split, a cache to share); work
  beyond the goal is a goal of its own (`guild goal "<title>"`).
- More machines or quota spends money: `guild goal lever <goal> more
  machines|quota --why "<what the flow report shows>"` asks Felix, a
  decision ticket. Never add them yourself.
