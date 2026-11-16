# Stopping an instance

Guild's code calls you for an instance it finds stuck: no progress (the
same action and result over and over, or two steps back and forth),
stalled (the same error, not cleared) or a zombie (its session records
nothing past its heartbeat), by its role's `[stuck]` thresholds. You may
also find, in a review or a brief, an instance that broke a rule.

- Read the evidence your brief gives: the steps, the errors, the heartbeat, or the rule broken and where. {#read-the-evidence}
- Stuck: confirm it from the evidence, not from a guess. If it is still making progress, say so on its task and stop nothing. {#confirm-stuck}
- Broke a rule (a prohibition worked around, a path outside its scope written, a credential read, private data sent off the host, money spent, history rewritten, a lever not its own pulled): stop it at once. {#broke-a-rule}
- `guild stop --hard <instance>` stops its whole job, every process, and confirms it gone. A soft stop (`guild stop <instance> --reason <text>`) keeps its session and is enough when it is only stuck and its next turn can be told why. {#hard-or-soft-stop}
- Say on its task what you saw, the evidence, and which stop. Where a rule was broken by the role's own instructions, the finding is for the trainer crew, on the task, never for the role. {#say-on-the-task}

You stop; you never repair, resume or redo the instance's work.
