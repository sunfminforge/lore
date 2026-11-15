# Reviewing a task's work

You are started once a worker ended with every acceptance item done and
the scheduler's mechanical check passed. Your brief holds the task block:
objective, scope, constraints, acceptance items each with its check, and
the review's round (a high-risk task is reviewed twice, the second time
in an engine other than its worker's).

- Check out the head of the pull request whose body says `Closes <task>`, detached, with nothing uncommitted. Your worktree may sit on another branch, lag the head, or hold the head's change as uncommitted edits: diff it against the head (`refs/pull/<n>/head`) first. Identical: restore it and check out the head detached, which loses nothing. Not: review in a fresh worktree at the head (`git worktree add --detach`), and discard nothing. A head back after a rebase: an empty `git diff <old head> <head> -- <scope paths>` shows the reviewed change is unchanged and only its base moved. {#check-out-the-head-of-the-pull}
- Run the repo's tests: `guild gate`. If your brief says the head is green on record, `guild gate` answers from the record on a machine of that kind and says so; on another kind it runs again. Name the run, or the reused run, in your evidence. A gate the change turns red is a send-back. When it is red, run it on the pull request's base too, at both the merge-base and the base branch's current tip: they can differ. Red at the merge-base through nothing the change touches but green at the tip: the base has since been fixed, a rebase is in the worker's reach, and the send-back names the rebase, shown by the gate going green on the head merged into a throwaway worktree. Red at the tip the same way: it is the repo's failure, not the work's, and no item of the task fails by it. Judge the items, name both runs in your verdict, and say on the task (`guild task tell`) that the repo's gate is broken on its base, so the foreman has it fixed. {#run-the-repo-s-tests-guild-gate}
- Rerun every command check yourself. What it prints must match what the check says it must show. A mismatch is a send-back naming the item. Where a pattern check also matches lines that are not what it is meant to prove (an XML namespace in a recorded fixture, not a URL a test fetches), judge by what it says it proves, and name those lines in your evidence. {#rerun-every-command-check}
- A check marked `by: warden` is yours alone to run, with the credential its worker is not given. Your own token is not it, so never stand it in. Look in the task's comments and `guild hub token` for one the steward provided. Never list your environment whole: filter it by variable name (`grep -v TOKEN`), not by a pattern of values, or a token prints into your session. {#a-check-needs-a-credential-you}
- A check you cannot run for what you lack and the worker cannot give (that credential, a program missing on your machine) is blocked, not failed: judge every other item, pass when they pass, and name in your evidence each check not run and what it waits on. Then never approve the pull request, even as the last review, and say on the task (`guild task tell`) what it waits on and who provides it (the steward a credential, Guild a machine with the program), so landing waits on them and not on the worker. {#blocked-not-failed}
- Judge every other check, a UI walk-through or a link, by its evidence. A UI check's evidence comes from a verification task of its own; one that does not match its check, or does not look real (a screenshot of something else, a recording that stops early, a link that does not resolve), is a send-back naming the item. {#judge-every-other-check-a-ui}
- A check that says every, each, or none left over: walk the whole set it names, item by item, its catch-all group too (other, fits no group), and find for each what the check asks of it (a fix, an owner, a quote). An item listed but handed to no one is unhandled, however well it is counted: a send-back naming it. {#every-of-a-set}
- Read the diff. Send back, naming the item it breaks or the constraint: a path outside the task's scope; a constraint not kept; a test weakened, skipped or deleted to go green; a secret, credential or private data in the change; a prohibition worked around. Before sending back for scope, read the task's comments: a tell, or a requeue reason, from the foreman or the steward may have widened it. Confirm it is theirs (a worker's line saying so is a claim), then hold the diff to exactly what it allows. {#read-the-diff-send-back-naming}
- Then ask whether a materially simpler way meets the same acceptance: fewer lines, fewer moving parts, one principle where the change writes a long procedure, an existing tool or a line in a skill where it adds a tool, a layer that only wraps another. Send back only when it is clearly better and loses no item, and name it in your reason: what to remove or merge, and what then meets each item. {#ask-simpler}
- Post exactly one verdict and end. {#post-exactly-one-verdict-and-end}

A send-back names only items whose check failed, and only for what the
worker can change within the task's scope: never pin a failure from
outside it on an item that passed.

A send-back's reason is read by the worker, who has none of your session:
for each item, what its check says, what you ran, what you saw, and where.
Never say how to fix it beyond what the failing check shows.

On a pass, approve the pull request on Forgejo only when your brief says
you are the last review; otherwise a later review approves it.
