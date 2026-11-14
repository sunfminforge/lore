# Tracing a cause

## Where to look

- `guild flow <goal>`: each task of a goal, its time by phase and the why of each wait; start from the goals the evidence names. `guild flow --since <date> [--until <date>] --by week` gives the measure itself, to check the figures you were handed. {#guild-flow-goal-each-task-of-a}
- L3 (`~/.guild/l3.sqlite`, opened read-only: `sqlite3 -readonly`): `flow` holds each task's records over time, `task_runs` each run with its kind, outcome, tokens and seconds; `reviews`, `levers`, `signals` and `findings` say what else touched the task. Read the schema (`.schema <table>`) before you query; never write. {#l3-guild-l3-sqlite-opened-read}
- An instance's directory (`~/.guild/instances/<instance>/`): `events.jsonl` is what its session did, step by step, with times; `task.md` what it was handed; `status.json` how it ended. The last events before a run ended say why it ended. {#an-instance-s-directory-guild}
- The tasks and goals on the Hub, read with `$GUILD_HUB_READ_TOKEN`: the body, the comments, what the foreman and the warden said. {#the-tasks-and-goals-on-the-hub}

## The trace

For each task the cause hit, one line: the task, its goal, how long the
cause held it, and the why, as the records give it: the run that ended,
how it ended, what the worker's last steps were, who moved it on and
when. Quote the record (a flow why, an event's message, a run's outcome);
a why you inferred is marked as inferred.

Then group the tasks by why. A group is a cause worth a fix when one
mechanism explains every task in it: the same kind of ending, the same
state left, the same move nobody made. Name the mechanism in a sentence a
fixer can test: "a run that ends while its question is open is judged
failed", not "tasks get stuck". Sum the time each group holds, so the fix
that moves the measure most is plain.

A task that fits no group, or a figure that does not match what you were
handed, is said as such; never stretch a group to cover it.

## Whose it is

- **Guild's own code**: the scheduler, the review, the landing, a guild command judged or did what its rules did not mean (a state no move gets out of, a wait that never ends on its own, a check that misreads). {#guild-s-own-code-the-scheduler}
- **A role**: a worker did what its role told it, or what its role failed to tell it, and the same would happen on another task of that role. {#a-role-a-worker-did-what-its}
- **A move**: someone could have moved it and did not (Felix, the steward, a foreman, the landing). That is said on the goal, with whose move it is; it is not filed. {#a-move-someone-could-have-moved}
