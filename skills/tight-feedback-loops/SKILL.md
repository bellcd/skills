---
name: tight-feedback-loops
description: Use when planning a chunk of work, verifying it with tests, or reporting a result.
---

# Tight Feedback Loops

Every rule here shortens the distance between doing something and knowing whether it worked.
Small slices, the smallest test scope that answers the question, evidence before assertions, and commands that run on one paste.

## Slice the work

Feature work ships as tracer bullets, ~never as one wide diff.

The first slice is the route plus the shell of the views, where the components will go.
Then one hard or risky part on its own. Then the rest, piece by piece.
Starting with the hardest part is fine, and often better.

Aim for a few hundred lines per slice, less for a shell.
When work is already built too wide, slice the existing files into sequential commits rather than presenting one large diff.

The house mechanism is the mattpocock plugin skills: `/implement`, which calls `/tdd` for vertical slices, and `/to-tickets` for tracer-bullet tickets.
They are user-invoked and won't self-fire: when told to start implementation work without one, suggest it, or follow the discipline anyway.

## Delete the superseded path

When a replacement approach wins, remove the old one. Don't propose "new mode by default, old mode still supported".

When scoping a migration, state plainly what the old path still buys and what the second mode costs in forked logic.
If the answer is one narrow use case already covered elsewhere, propose deletion.

Name the incidental cruft the deletion also removes. It strengthens the case, and it stops that cruft surviving by inertia.

## Verify before asserting

Before claiming how code behaves, especially under failure or misuse, read the implementation.
Don't only reason from first principles when the configuration is observable.

Find the knob that actually decides the behavior, then quote it.

A metric can exclude the very thing you are checking, so a good number is not yet a verified claim.
Read what a metric leaves out before quoting it as evidence.

Cumulative Layout Shift is a clear case. It drops every shift within half a second of a click,
so a surface that jumps on every control flip still measures near zero. A prototype once reported 0.001 while visibly moving on each change.
Interaction stability needs raw geometry instead, bounding rectangles compared before and after. CLS answers for load.

## Run the smallest scope that answers the question

While diagnosing, run the fewest tests that resolve the current question, e.g. a single spec file, a single directory, explicit worker count. Not the full suite.

Escalate scope and concurrency one variable at a time.

A full-suite run is the user's to launch, or needs asking first. That includes a run whose only purpose is confirmation.

## Final verification uses the plain commands

Env prefixes and reporter flags are fine inside a diagnosis loop. The run that declares a suite green is the repo's bare script (e.g. npm script), with no decoration.

The suite has to pass the way the user and CI invoke it.

Use a fresh login shell, so profile changes the session's shell snapshot predates are picked up.

## Hand over commands as one line

A command given to the user is a single unbroken line.
They run it directly, so anything split costs reassembly.

Absolute paths inline, one fenced block, one line.
Don't wrap long paths onto continuation lines, don't split a `cd` off from the command it sets up, and don't offer the same command twice as competing variants.
