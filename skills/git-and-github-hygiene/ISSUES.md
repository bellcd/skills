# GitHub Issues

Tracker content is canonical once posted and drives downstream work,
so Christian reviews every issue mutation the way he reviews code.
Nothing reaches the tracker unreviewed.

## Drafting a new issue

Write the draft as a repo file — `docs/prd/<slug>.md` or similar — rather than presenting it only in the conversation.
Edits during review rounds then show up as git diffs, instead of forcing a re-read of the whole document each round.

Present the draft, wait for explicit approval, then post with `gh issue create --body-file`.
For a batch, show every draft before posting any of them.

Ask whether the issue belongs on a GitHub Project as part of that approval question.
`gh project list --owner <org>` for the candidates, `gh project item-add <number> --owner <org> --url <issue-url>` to place it.

## Editing a published issue

Fetch the current text to a local file, apply the change locally, and show the diff.
Push with `gh issue edit --body-file` only after approval.

```bash
gh issue view <n> --json body --jq .body > old.md
cp old.md new.md   # edit new.md
diff -u old.md new.md
```

**Hand over the command as well as its output.** When a diff is between files outside the
local repo — print the exact command Christian can run himself alongside the inline diff, using absolute paths.
He re-runs these in his own pager. `git diff --no-index <a> <b>` for colour, `--word-diff` for prose edits.

## Issues carry signal only

Implementation decisions, UI-design choices, and slice plans do not go in issue comments.
That's noise, and it buries what a human actually needs.

Before drafting a comment, ask whether it changes what a human has to do or verify:

- **A real requirement** (e.g. a telemetry event) → an acceptance-criterion edit to the body, through the diff-first approval flow above.
- **Everything else** → `NOTE:` and `TODO:` comments in code, prototypes, pull-request review.

## Recording a completed spike

The issue is the record, not a committed findings file — a parallel `.md` is duplication that drifts.

1. Tick the acceptance-criteria boxes. That is the _only_ edit to those lines: no annotation of the criterion text, no inlined verdict.
2. Add a **minimal** comment for the findings — approach pivots, verdicts, friction. Terse, not an essay.

A repo doc may hold a runbook of manual repro steps, plus a pointer to the issue.

## Closing an issue

Two edits to the body, both in the same push:

1. **Tick every acceptance criterion that is actually done.** Leave genuinely unmet ones unticked and say so.
2. **Append a `## Time spent` section** — `~N h of dev time, YYYY-MM-DD to YYYY-MM-DD`.

Do this as part of closing, unprompted. Nothing records hours automatically, so a figure not written at close is gone.
An estimate is fine — a rough number beats no number.

Reconstruct from local Claude Code session transcripts when the work is recent.
Sum the gaps between consecutive transcript events inside the work's window, capping any gap over 15 minutes.
Otherwise estimate from the wall-clock span and say that's what it is.

A body section, not a comment and not a project field.
