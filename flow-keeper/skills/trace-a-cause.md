# Tracing a cause

## Where to look

- `guild flow <goal>`: each task of a goal, its time by phase and the why of each wait; start from the goals the evidence names. `guild flow --since <date> [--until <date>] --by week` gives the measure itself, to check the figures you were handed. {#guild-flow-goal-each-task-of-a}
- L3 (`~/.guild/l3.sqlite`, opened read-only: `sqlite3 -readonly`): `flow` holds each task's records over time, `task_runs` each run with its kind, outcome, tokens and seconds; `reviews`, `levers`, `signals` and `findings` say what else touched the task. Read the schema (`.schema <table>`) before you query; never write. {#l3-guild-l3-sqlite-opened-read}
- An instance's directory (`~/.guild/instances/<instance>/`): `events.jsonl` is what its session did, step by step, with times; `task.md` what it was handed; `status.json` how it ended. The last events before a run ended say why it ended. {#an-instance-s-directory-guild}
- The daemon's own logs (`~/.guild/guildd.log`, the scheduler's log): what could not start and why, what the landing refused, and which instances started in each round; a flow why names a wait, these say what held it. Read a setting the trace turns on, such as the round's length, from its configuration, never assume it. {#the-daemon-s-own-logs}
- The tasks and goals on the Hub, read with `$GUILD_HUB_READ_TOKEN`: the body, the comments, what the foreman and the warden said. `gh` does not reach the Hub: say what you found on your goal with `guild tool forge-comment say`, which signs in as your own account and never shows its token. {#the-tasks-and-goals-on-the-hub}

## The trace, as a diagnosis loop

Run the trace as Matt Pocock's diagnosing-bugs loop (MIT; the pull request
that adopted it pins the commit), turned from a bug in code to a measure
in records: the figure stands where the failing test stood.

- Figure first: one `guild flow` command or read-only query, already run, that prints the figure you were handed (the measure over its window, the hours the cause held), the same on every run. A theory of why, read off a log before that command exists, is the guess the loop is there to stop. Where it prints another figure, that gap is the first thing you say: a brief's figures drift as goals end (`guild flow` runs a task's last wait to its closure, L3's `flow` records do not), and a figure the brief names may have no command that prints it at all; find which before you promise to reproduce it. {#figure-first}
- Narrow it: take tasks, goals and days out of the query one at a time, rerunning it after each, until every task left carries part of the cause's hours; those are the tasks the cause hit. {#narrow-to-the-tasks-it-hit}
- Count each hour once, so the trace sums to the flow figure: a wait on another task re-books that task's own phases, so set each dependent's wait beside its predecessor's queued, working, review and landing times; under a goal, count only its own tasks' lines, never the copies nested from the tasks they wait on. {#count-each-hour-once}

For each of them, one line: the task, its goal, how long the cause held
it, and the why, as the records give it: the run that ended, how it
ended, what the worker's last steps were, who moved it on and when. Quote
the record (a flow why, an event's message, a run's outcome); a why you
inferred is marked as inferred.

- Before you settle on why, write three to five mechanisms that could hold the tasks, ranked, each with what it predicts in the records ("if runs end while a question is open, each run's last event is that question"); one that predicts nothing a record can show is dropped. Test them in rank order, one query each. A flow why is a label to test, never a mechanism: one label can hold several (a round wait during which instances did start, a failure flow gives no reason for), and two always on the list are the measure misread (recompute it pooled, from corrected inputs) and a cause already fixed (its fix's landing against the span), both tested before you blame the machines or the queue's order. {#rank-mechanisms-by-prediction}
- A mechanism is a cause worth a fix when its prediction holds on every task of its group: the same kind of ending, the same state left, the same move nobody made. Name it in a sentence a fixer can test: "a run that ends while its question is open is judged failed", not "tasks get stuck". Sum the hours each group holds, split by each machine's own reason and cut where a ticket that ended it closed, so the fix that moves the measure most is plain; a sum that mixes them overstates what the fix can win. {#a-mechanism-holds-on-every-task}
- The query that showed a mechanism goes into its ticket as the check of its acceptance: run after the fix, it prints the group's hours gone. Where no record can show the mechanism (nothing logged at that step), that is a why of its own: Guild's code lacks the record, and it gets a ticket too. {#the-query-is-the-check}

A task that fits no group, or a figure that does not match what you were
handed, is said as such; never stretch a group to cover it. The trace is
done when the figure is reproduced or its gap said, each task is in a
group or said as fitting none, and each mechanism ruled out is named with
the record that ruled it out.

## Whose it is

- **Guild's own code**: the scheduler, the review, the landing, a guild command judged or did what its rules did not mean (a state no move gets out of, a wait that never ends on its own, a check that misreads). {#guild-s-own-code-the-scheduler}
- **A role**: a worker did what its role told it, or what its role failed to tell it, and the same would happen on another task of that role. {#a-role-a-worker-did-what-its}
- **A move**: someone could have moved it and did not (Felix, the steward, a foreman, the landing). That is said on the goal, with whose move it is; it is not filed. {#a-move-someone-could-have-moved}
