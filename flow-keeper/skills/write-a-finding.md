# Writing a finding for the trainer crew

A cause that is a role, how an instance or the foreman worked, is the
trainer crew's to fix, by a change to that role. You never correct the
role or its instances yourself.

Write it in the shape of a detector's finding:

```toml
[[finding]]
role = "<the role>"
saw = "<what the role did, as a fact, and what it cost the flow>"
evidence = ["<task>: <the wait, and the line or row that shows it>", …]
```

- `saw` names the way of working, not the incident: what the role did that held tasks, true across the tasks it hit. {#saw-names-the-way-of-working-not}
- The same way seen in several roles is one finding, `role` naming them all: it is the level above them the crew may change. {#the-same-way-seen-in-several}
- Post it on your goal, addressed to the trainer crew, with what else it explains in the measure. Never on the role's own tasks or to its instances. {#post-it-on-your-goal-addressed}
