# Filing the fix

## The whys as a batch

Take the trace's groups together, as Matt Pocock's retro skill (MIT;
pinned in the adopting pull request) takes a session, and settle where
each fix holds best before you look for a ticket:

- A why true of one task alone, which says nothing of how Guild's code or a role works, is said on the goal and filed nowhere. {#one-task-alone-is-said}
- A role's why that Guild's code could refuse or report (a field left empty, a step skipped that a command can check) is a ticket for that check, not a finding asking the role to remember. {#a-check-over-a-reminder}
- A worker's slip the review let pass is a finding on the reviewing role as well; time lost searching for the task's own state is a ticket for the command that would have shown it. {#the-reviewer-and-the-lookup}

## Look before you file

For each cause, search the tracker of Guild's code (sunfmin/control) for
an open ticket that covers it: `gh issue list -R <repo> --state open
--search "<words of the mechanism>"`, then read each candidate and any
the task names (`gh issue view <n> -R <repo> --comments`). A ticket
covers a cause when its fix would end the cause for every task you
traced; one that covers part of it covers that part.

- Covered: comment on that ticket with the new evidence, the tasks, the time and how they bear on it. Never open a second. {#covered-comment-on-that-ticket}
- Partly covered: comment on it for its part; file only what is left, and say in the new ticket which ticket holds the rest. {#partly-covered-comment-on-it-for}
- Closed as fixed: it covers only the waits that ended before its pull request merged, so read that merge time (`gh pr view`), not the ticket's date nor an earlier fix's. The waits after it are what remains: a new ticket that links it and says what the fix did not reach, filed however small the remainder. {#closed-as-fixed-and-the-cause}

## A ticket

One ticket for what is left, labelled for the guild fleet as the task or
goal says. It holds:

- **What happens**: the mechanism, in a sentence a test can check. {#what-happens-the-mechanism-in-a}
- **Evidence**: each task, its goal, the time the cause held it and the record that shows it, linked where a reviewer on another host can open it (the Hub or GitHub comment, the ticket), never only a note in your worktree; the measure and its figures over the window. {#evidence-each-task-its-goal-the}
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

One comment on your goal: each cause, the one holding the most hours
first, its time, and what you filed for it
(the ticket's link, the comment's on an existing one, the finding), and
each move that is someone's, with whose it is. On a send-back, revise
that comment in place rather than adding another.

- Before you check off an item, every why you placed has its mark: a ticket, a comment on one, a finding, or whose move it is; and each link you cite is opened and read to say what you quote of it, not only that it exists. {#every-why-has-its-mark}
