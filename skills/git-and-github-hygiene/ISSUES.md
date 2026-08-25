# GitHub Issues

Tracker content is canonical once posted and drives downstream work,
so the user reviews every issue mutation the way they review code.
Nothing reaches the tracker unreviewed.

## Drafting a new issue

Write the draft as a repo file, `docs/prd/<slug>.md` or similar, rather than presenting it only in the conversation.
Edits during review rounds then show up as git diffs, instead of forcing a re-read of the whole document each round.

Present the draft, wait for explicit approval, then post with `gh issue create --body-file`.
For a batch, show every draft before posting any of them.

**Never use AskUserQuestion to approve content.** It swallows the prose written before it, so a draft
presented in the same turn is invisible and the user is choosing blind. Approval goes in a plain
final text message with the content inline. Reserve the question tool for scope, timing, or
field-only choices where the options stand on their own.

**Don't ask anyone to pick from a list they cannot see.** Offering three named batches of work is not
a question until the raw material is on the table. Show the titles, then ask.

Ask whether the issue belongs on a GitHub Project as part of that approval question.
`gh project list --owner <org>` for the candidates, `gh project item-add <number> --owner <org> --url <issue-url>` to place it.

Project commands need a scope the default login does not carry. `gh auth refresh -s project` once, or every call fails on permissions rather than on anything to do with the project.

## Editing a published issue

Fetch the current text to a local file, apply the change locally, and show the diff.
Push with `gh issue edit --body-file` only after approval.

```bash
gh issue view <n> --json body --jq .body > old.md
cp old.md new.md   # edit new.md
diff -u old.md new.md
```

**Hand over the command as well as its output.** When a diff is between files outside the
local repo, print the exact command the user can run themselves alongside the inline diff, using absolute paths.
They re-run these in their own pager. `git diff --no-index <a> <b>` for colour, `--word-diff` for prose edits.

## Issues carry signal only

Implementation decisions, UI-design choices, and slice plans do not go in issue comments.
That's noise, and it buries what a human actually needs.

Before drafting a comment, ask whether it changes what a human has to do or verify:

- **A real requirement** (e.g. a telemetry event) → an acceptance-criterion edit to the body, through the diff-first approval flow above.
- **Everything else** → `NOTE:` and `TODO:` comments in code, prototypes, pull-request review.

## Recording a completed spike

The issue is the record, not a committed findings file. A parallel `.md` is duplication that drifts.

1. Tick the acceptance-criteria boxes. That is the _only_ edit to those lines: no annotation of the criterion text, no inlined verdict.
2. Add a **minimal** comment for the findings: approach pivots, verdicts, friction. Terse, not an essay.

A repo doc may hold a runbook of manual repro steps, plus a pointer to the issue.

## Closing an issue

Two edits to the body, both in the same push:

1. **Tick every acceptance criterion that is actually done.** Leave genuinely unmet ones unticked and say so.
2. **Append a `## Time spent` section**: `~N h of dev time, YYYY-MM-DD to YYYY-MM-DD`.

Do this as part of closing, unprompted. Nothing records hours automatically, so a figure not written at close is gone.
An estimate is fine. A rough number beats no number.

Reconstruct from local Claude Code session transcripts when the work is recent.
Sum the gaps between consecutive transcript events inside the work's window, capping any gap over 15 minutes.
Otherwise estimate from the wall-clock span and say that's what it is.

Keep only the sessions whose subject matter is the issue, judged from their opening prompts. Matching on the issue number alone over-counts badly, since a number can be named in passing by any session.

An AFK run is invisible to this method, because its transcript lives on the sandbox VM rather than locally. What the figure measures is host-side dispatch and review time, so a realized-against-budget comparison is human hours against human-plus-agent output.

<!-- TODO: nothing surfaces that gap at the point of reading. A `## Time spent` figure looks
     like total effort, and silently is not, so the more AFK work an issue carried the more it
     understates. Options: pull the VM transcripts back to the host and fold them in, label the
     figure in the issue body as host-side only, or state the gap when reporting a number that
     covers AFK work. Pick one rather than leaving the reader to know this. -->

A body section, not a comment and not a project field.
