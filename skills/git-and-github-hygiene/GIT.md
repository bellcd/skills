# Branches, Commits, and Pushes

## Never change the working tree while on main

Branch first. This covers every repo change, including file-mode bits like `chmod +x`.

When a fix is needed mid-task while on main, propose alternatives. E.g. branching, worktrees, stashing, invocation workaround, etc.

## Related spikes share one branch

When several spikes are sub-questions of one overarching question, keep them on a single long-lived branch rather than merging each one into main.
Open the pull request only once the overarching question is settled.

That PR's diff is then the curated keep, and whatever stays behind on the branch is self-evidently throwaway.

## Prototype variants stay out of the feature commit

A throwaway branch for the variants is fine.
Prototype files must not ride along into the feature commit. Check `git status` before committing, since anything sitting in the index goes with it.

The winning decision is what gets folded into the real code.

## Commits wait for an explicit go-ahead

Every slice stays uncommitted until the user has reviewed the working-tree diff and said to commit.
"Separate commits" means separate review-then-commit cycles, not committing slice by slice unprompted.
Build the slice, leave it uncommitted, say it's ready for review, wait. Commit on the go-ahead, then start the next slice.

This holds in every repository, the skills repo and any personal checkout included. The rule is about the review happening, not about which project the work sits in.

A go-ahead covers the slice it was given for.
It does not extend to a second repository touched in the same turn, and a fix discovered afterwards, e.g. during live verification, does not inherit it.
Leave that fix in the working tree, hand over the re-verification commands, and commit only once the check has passed.

## Review the change before presenting it

Run the two-axis code-review skill on a code change before handing the working tree over, reviewing against the merge-base with `main` for branch work.
This is the default for every change, with exceptions, rather than something reserved for PRs.

Report the findings alongside the change, including the ones deliberately not acted on and why.

The user's review time is the scarce resource.
Defects a review would have caught shouldn't be spent on a human. Arriving with the review already done makes their pass about judgment rather than defect-hunting.

## Green precedes the commit, not just the pull request

For environment and infrastructure changes, the suite passes _before_ the commit is made.
Don't commit staged infra work and verify afterwards. Stage everything, hand over the run commands, wait for confirmation.

The acceptance bar differs by the kind of work:

- **Implementation or migration**: the entire local end-to-end suite green, accounting for every provider the suite exercises.
- **Spike**: a single representative outcome, e.g. one end-to-end test passing through the new path. Spikes stay cheap.

## Push once, when the branch is review-ready

Every push to a pull-request branch can trigger CI, and CI minutes might be constrained.
Accumulate commits locally and push once, rather than pushing per commit. Before any push confirm it's wanted now or clearly necessary.

Run the local verification before pushing: unit tests, type-check, lint. A push shouldn't be spent discovering red CI.

While the user is actively reviewing a branch, don't push at all.
Batch the local work and push once they say the review is done.
