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
- Merge or re-split tasks still queued, never one running or in review:
  a split file with `replaces: [<task>, …]`, proposed with
  `guild goal split <goal> <file>`. The new tasks pass the split rules,
  side by side with the tasks the goal keeps too; the replaced ones are
  closed, saying which replaced them. Replace a task's verification task
  with it.
- A task that failed before it started (its flow says it could not
  start: a repo not found, say) fails again as it is: read why in its
  flow, then re-split it so it can start, or leave it out and say on the
  goal what it waits for.
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
