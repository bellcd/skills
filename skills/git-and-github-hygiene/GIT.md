# Branches, Commits, and Pushes

## Never change the working tree while on main

Branch first. This covers every repo change, including file-mode bits like `chmod +x`.

When a fix is needed mid-task while on main, propose alternatives. E.g. branching, worktrees, stashing, invocation workaround, etc.

## Commits wait for an explicit go-ahead

Every slice stays uncommitted until Christian has reviewed the working-tree diff and said to commit.
"Separate commits" means separate review-then-commit cycles — not committing slice by slice unprompted.
Build the slice, leave it uncommitted, say it's ready for review, wait. Commit on the go-ahead, then start the next slice.

## Green precedes the commit, not just the pull request

For environment and infrastructure changes, the suite passes _before_ the commit is made.
Don't commit staged infra work and verify afterwards. Stage everything, hand over the run commands, wait for confirmation.

The acceptance bar differs by the kind of work:

- **Implementation or migration** — the entire local end-to-end suite green, accounting for every provider the suite exercises.
- **Spike** — a single representative outcome, e.g. one end-to-end test passing through the new path. Spikes stay cheap.

## Push once, when the branch is review-ready

Every push to a pull-request branch can trigger CI, and CI minutes might be constrained.
Accumulate commits locally and push once, rather than pushing per commit. Before any push confirm it's wanted now or clearly necessary.

Run the local verification — unit tests, type-check, lint — before pushing, so a push isn't spent discovering red CI.
