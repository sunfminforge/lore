# Splitting a goal

The scheduler starts you on a goal with a brief: the goal, the roles lore has,
the roles drafted in lore's open pull requests, and the split rules.

- Split into as many tasks as the goal needs: no rule limits how many; the
  machines bound how many run at once. A small goal is one task.
- Split by independent artifacts: two tasks that may run side by side change
  different paths. A coupled design is one task, or tasks in order (`after`).
  A task `after` another starts only once that one lands, not once it
  passes review: order tasks only when the later one needs the earlier
  one's change landed.
- Split only work that can start now. Scope each task only in repos the
  forge has: a task in a repo the scheduler cannot clone fails before it
  starts. Check every repo of the split with `git ls-remote
  <forge>/guild/<repo>.git` first (the `origin` of lore shows the forge);
  the brief's example repo is an example, not a repo you have. Work that
  builds on what has not landed (another goal's task: `after` names only
  tasks of the same split) cannot start either: a constraint telling the
  worker to wait only parks it and fails the task. Leave such a piece out
  of the split, a repo to be made (`guild repo new`, on the Hub, not
  yours to run) or work of another goal, and say on the goal what it
  waits for and whose move that is; split it once it has landed.
- Give each task a `priority` within the goal's: the critical path
  highest, work that can wait lower. A low goal's tasks are low: they wait
  for an idle machine or a spare account, hours on a busy one; the goal's
  priority is Felix's, never raised by a task's.
- Give each task a role lore has, or one drafted in lore. When none fits,
  ask for the role: list it under `requests` (its `role`, the `work` it
  does, its `constraints`) and give the task that role. The scheduler opens
  the role request as a task for the trainer crew; the task waits until the
  role lands.
- While lore has no warden, the split is refused: the trainer drafts it
  first.
- Give every acceptance item its check, how it is verified: a `command` and
  what it must show, which the warden reruns; a `ui` walk-through and what
  must be seen; or a `link`. An item with none refuses the split.
- A check that needs more than a command the warden can rerun, a UI tried
  for real or a recording, gets a verification task of its own: after the
  change alone, its `verifies` naming the items it walks, for a verification
  role other than the change's (a UI tester; ask for it when none fits,
  with the capabilities it needs), changing nothing the change changes. It hands
  in evidence; the warden judges it in the change's review.

Write the split to a file and run `guild goal split <goal> <file>`. It exits 0
once the scheduler opened the tasks, and 1 with every reason it refused them:
change the split and propose it again. Once it is accepted, say on the goal
what any task of it waits for before it can run (an idle machine, a role,
another task) and whose move that is; then end.
