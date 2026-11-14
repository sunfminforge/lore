# Filing the fix

## Look before you file

For each cause, search the tracker of Guild's code (sunfmin/control) for
an open ticket that covers it: `gh issue list -R <repo> --state open
--search "<words of the mechanism>"`, then read each candidate and any
the task names (`gh issue view <n> -R <repo> --comments`). A ticket
covers a cause when its fix would end the cause for every task you
traced; one that covers part of it covers that part.

- Covered: comment on that ticket with the new evidence, the tasks, the time and how they bear on it. Never open a second. {#covered-comment-on-that-ticket}
- Partly covered: comment on it for its part; file only what is left, and say in the new ticket which ticket holds the rest. {#partly-covered-comment-on-it-for}
- Closed as fixed, and the cause came back: a new ticket that links it and says what the fix did not reach. {#closed-as-fixed-and-the-cause}

## A ticket

One ticket for what is left, labelled for the guild fleet as the task or
goal says. It holds:

- **What happens**: the mechanism, in a sentence a test can check. {#what-happens-the-mechanism-in-a}
- **Evidence**: each task, its goal, the time the cause held it and the record that shows it; the measure and its figures over the window. {#evidence-each-task-its-goal-the}
- **What should happen instead**: per part, the behaviour wanted, never the code to write. {#what-should-happen-instead-per}
- **Acceptance**: criteria a test can check, one per part. {#acceptance-criteria-a-test-can}
- **Related**: the tickets it relies on or splits from. {#related-the-tickets-it-relies-on}

No token, no secret, no person's private data in it; instances and
tasks by name, people by role.

## A finding

Where the cause is a role, write a finding for the trainer crew on your
goal, in the crew's form:

```toml
[[finding]]
role = "<role>"
saw = "<what the role did, as a way of working, in a sentence>"
evidence = ["<task>: <the record that shows it>", "…"]
```

The same finding on several roles says so: it is one for the level above
them. Never write to the role, its lore or its instance.

## On the goal

One comment on your goal: each cause, its time, and what you filed for it
(the ticket's link, the comment's on an existing one, the finding), and
each move that is someone's, with whose it is. Then check off your
task's items with the links as evidence.
