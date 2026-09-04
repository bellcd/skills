# Pull Requests

## Start from the repo's template

Before writing a body, look for a pull request template in `.github/`, `docs/`, or the repository root. Where one exists it sets the body shape for that repo, and everything below applies on top of it.

Fill in the sections that apply and delete the rest. A section the change has nothing to say for comes out whole, and so do the template's own guidance comments.

An agent never meets the template by accident. `gh pr create` skips it whenever a body is supplied by flag, which is every agent PR, so reading the file is the only way it reaches one.

## Bodies stay short

A few sentences: what the slice does and `Refs #n`. Nothing more.

No detail-by-detail recap of the change, and explicitly no list of what a code-review pass altered.
The diff and the commits are the record. Bodies that carry more detail than that go unread.

Do not include text about what tests were or were not run. That is noise. CI should always run tests.

## Closing keywords key on when verification happens

Use a closing keyword (`Closes #n`) only when every acceptance criterion is verified _before_ the merge. Tick the criteria checkboxes in the issue body at the same time.

When the fix can only be confirmed _after_ it merges, reference the issue non-closingly instead: `(#n)` in the subject, `Refs #n` in the body.
Add a short Verification section saying how and when confirmation happens, and leave the issue open for the user to close once it does.

A workflow change is the standard case. A `repository_dispatch` workflow runs from the default branch's copy of itself, so the pull request's own run exercises the old version and proves nothing about the change.
