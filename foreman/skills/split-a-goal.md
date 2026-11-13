# Splitting a goal

The scheduler starts you on a goal with a brief: the goal, the roles lore has,
the roles drafted in lore's open pull requests, and the split rules.

- Split into as many tasks as the goal needs: no rule limits how many; the
  machines bound how many run at once. A small goal is one task.
- Split by independent artifacts: two tasks that may run side by side change
  different paths. A coupled design is one task, or tasks in order (`after`).
- Scope each task only in repos the forge has: a task in a repo the
  scheduler cannot clone fails before it starts. Check every repo of the
  split with `git ls-remote <forge>/guild/<repo>.git` first (the `origin`
  of lore shows the forge); the brief's example repo is an example, not a
  repo you have. When the goal needs a repo that does not exist, give no
  task a scope in it: say on the goal that it must be made first (`guild
  repo new`, on the Hub, not yours to run) and leave that piece out of
  the split until it exists.
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
change the split and propose it again. Once it is accepted, end.
