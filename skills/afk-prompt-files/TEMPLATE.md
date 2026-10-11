# Prompt template for a ticket-to-PR run

Copy everything below the rule. Fill each `<!-- slot -->` with what it asks for and delete the comment. Keep every other line as written.

---

<!-- slot: goal. One sentence: the change, the ticket key, and "one draft PR". -->

Nobody is watching this run. Finish the work, or stop at a blocker you cannot get past. Either way, end by printing the report described at the bottom.

Your first turn starts with commands, not a statement of intent. A turn that ends with no tool call and nothing pending ends the run.

Your first command invokes `/<!-- slot: the repo's skill for this VM, e.g. afk-cloud -->`. Follow it for the whole run. Every subagent prompt you write tells the subagent to read that skill first.

If a skill named here is missing from your roster, find its `SKILL.md` with `find ~/.claude/plugins/cache .claude/skills -path '*/<name>/SKILL.md'` and follow it.

## Ticket

<!-- slot: ticket. Summary, problem and acceptance criteria, verbatim. -->

## Context

<!-- slot: context. Files to read first, the precedent to copy, traps specific to this change. Decide any product call the ticket leaves open here, and say it goes in the PR body. -->

## Scope

<!-- slot: scope. The files the change may touch. A needed edit outside them is a finding for the report, not an edit. -->

## Setup

`git fetch origin`, then `git switch -c <!-- slot: branch, claude/<key>-<slug> --> origin/main`.

## Test first

Use `/mattpocock-skills:tdd`. Show each new test red against `origin/main` by applying a test-only patch there, not with `git stash`.

<!-- slot: tests. The tier and spec file each criterion lands in. The scoped command nearest the change, and its result on origin/main. -->

## Verification

<!-- slot: verification. Tiers to run and tiers to skip, each skip with its reason. -->

Every assertion that something is absent sits beside one that it is present.

## Review

Run `/mattpocock-skills:code-review` on the branch diff against `origin/main`, both axes, with the Ticket section as the spec. Apply the findings.

## Finish

<!-- slot: lines the VM skill says every prompt must carry, e.g. refusing the cloud stop hook's commit request until the review is applied. -->

Commit, push, and write the PR per `/bellcd-skills:git-and-github-hygiene`. Open it as a draft.

<!-- slot: PR extras, e.g. a visual review section. Delete if none. -->

## Report

Print this and stop:

- The PR URL
- Each acceptance criterion, met or not, with its evidence
- Each verification command and its result
- Every failing test and every review finding not applied, by name. Never summarize one away
- Anything that could not run here
