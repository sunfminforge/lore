# Tracing a wait to its cause

## Measure

- `guild flow --since <date> [--until <date>] [--repo <r>] [--role <r>] --by day --table` shows the phases (queued, working, review wait, review, landing queue) and the biggest waits by cause, with how many tasks and goals each hit. `guild flow <goal>` shows one goal. {#guild-flow-since-date-until-date}
- Take the measure your goal names, in its window. Note its value now: the fix is judged against it. {#take-the-measure-your-goal-names}

## Find the tasks it hit

- L3 is `~/.guild/l3.sqlite`; open it only with `sqlite3 -readonly`. `flow` holds each task's records (`task`, `at`, `record` as JSON); `task_runs`, `reviews`, `came_back`, `levers`, `slow_phases`, `steps` and `loops` say what happened around them. Read `.schema` first; never write. {#l3-is-guild-l3-sqlite-open-it}
- From the records, list each task the cause hit, with how long it waited and from when to when. {#from-the-records-list-each-task}

## Find why each waited

- For each task, read what its instances saw in that span: under `~/.guild/instances/<role>-<id>/`, `events.jsonl`, `status.json`, `job.log` and `terminal.log`; and `~/.guild/scheduler.log` and `guildd.log` for the scheduler's and guildd's side. {#for-each-task-read-what-its}
- Read the issue and pull requests on the Hub, with $GUILD_HUB_READ_TOKEN, for what was said. {#read-the-issue-and-pull-requests}
- Write the mechanism in one sentence: what held the task, what would have let it go. Several tasks with one mechanism are one cause; one task held by two mechanisms is two. {#write-the-mechanism-in-one}

## Sort by owner

- **Guild's code**: the scheduler, guildd, landing, admission, a `guild` command did or failed to do what would have let the task go. A ticket. {#guild-s-code-the-scheduler}
- **A role**: an instance or the foreman worked a way its role told it, or failed to know (a split that chained tasks needlessly, a hand-in the warden keeps sending back for the same thing). A finding. {#a-role-an-instance-or-the}
- **Neither**: a person's decision awaited, a machine down, a one-off. Say it on the goal with its evidence; file nothing. {#neither-a-person-s-decision}
- A cause you could not trace to a mechanism is said so on the goal, with what you looked at; it is not filed. {#a-cause-you-could-not-trace-to-a}
