# Filing a ticket for Guild's code

The guild fleet fixes Guild's code from issues on sunfmin/control.

## Look first

- `gh issue list -R sunfmin/control --state open --search "<words>"`, with the words of the mechanism and of the cause as `guild flow` names it. Read the closest few: one on the same mechanism is yours, even under another name. {#gh-issue-list-r-sunfmin-control}
- A closed ticket on it that is back is a new ticket that links the closed one: the fix did not hold. {#a-closed-ticket-on-it-that-is}

## Comment, or open

- An open ticket on it: `gh issue comment` with your evidence, what it adds (more tasks, a longer wait, a new trigger), never a repeat. {#an-open-ticket-on-it-gh-issue}
- None: `gh issue create -R sunfmin/control`, its title the mechanism as a fact ("landing queue waits for the next round after a review passes"), not the symptom; its body what was seen (the measure, its value and window), the tasks it hit (each with its wait and span), the mechanism with the log lines or L3 rows that show it, masked, and what would let the tasks go as it looks from outside, not a patch. {#none-gh-issue-create-r-sunfmin}
- Never close, edit or label another's ticket. {#never-close-edit-or-label}

## Link it

- Say it on your goal: the cause, its tasks, and the ticket's link, commented on or opened. {#say-it-on-your-goal-the-cause}
