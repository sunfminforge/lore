# Answering a signal

A signal counts what went wrong on a goal; its comments say why. Read why
first, then correct the foreman, change its role, or both.

- Before you act on a signal, read the comments of its goal and of the tasks it names (`guild tool forge-read issue <repo> <n> --comments`): the refusals' reasons, the holds and the send-backs name the cause better than the signal's count does. {#read-the-reasons}
- Count the crew's corrections on the goal so far (its `<!--guild:correction-->` comments) before planning one, and read the goal's newest tasks: the cap is per goal, an earlier instance may already have sent the one you would, and the foreman may already have re-split around the failure, which leaves nothing to correct. {#count-corrections}
- Split refused for reasons that are task states, not the cut: the foreman retried a wait, it does not split badly. Correct it only when the blocker has just cleared and it may not have heard: say what changed, and to propose once. {#refused-on-a-wait}
- Stalled on a queued task that waits on a held or failed one: the hold usually names a dependency on another goal that the foreman dispatched too early; what to fix is how its split orders work across goals. {#stalled-on-a-hold}
- Tasks failed or sent back with green work, or one failing on a single item of many: look for an item nobody it names can meet, an acceptance check needing a credential nobody holds, or a state true only after the session (a replay, a landing); correct the foreman to re-split once without it, and fix its split rule. {#check-nobody-can-run}
- Back and forth: read the goal's re-splits in order, not only the last; the avoidable one is often earlier (around a steward hold, or another goal's moving work). Find in the replaced tasks' comments the assumption that split made (who holds a credential, say), and fix how the foreman's role knows it, not only that one outcome. {#back-and-forth}
