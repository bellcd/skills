# Pull Requests

## Start from the repo's template

Before writing a body, look for a pull request template in `.github/`, `docs/`, or the repository root. Where one exists it sets the body shape for that repo, and everything below applies on top of it.

Fill in the sections that apply and delete the rest. A section the change has nothing to say for comes out whole, and so do the template's own guidance comments.

An agent never meets the template by accident. `gh pr create` skips it whenever a body is supplied by flag, which is every agent PR, so reading the file is the only way it reaches one.

## The title carries the ticket key

End the title with the key in parentheses, uppercase:

```bash
gh pr create --title 'Build the narrative from an ordered list of cards (PT-191)' --body-file <path>
```

Jira matches the key case-sensitively, so a lowercase key links nothing. The title is what puts the pull request in that ticket's Development panel.

## Bodies stay short

A few sentences on what the slice does, then the reference:

```
Refs [PT-nnn](https://parlancetherapy.atlassian.net/browse/PT-nnn)
```

Written out in full, because GitHub does not linkify a Jira key. That host is the Parlance tracker. Nothing more belongs in the body.

No detail-by-detail recap of the change, and explicitly no list of what a code-review pass altered.
The diff and the commits are the record. Bodies that carry more detail than that go unread.

Do not include text about what tests were or were not run. That is noise. CI should always run tests.

## Move the ticket only after verification

Moving a Jira ticket is always a separate act from the merge. Tick the acceptance criteria and move it to Done only when every one is verified _before_ the merge.

When the fix can only be confirmed _after_ it merges, add a short Verification section saying how and when confirmation happens, and leave the ticket where it is for the user to move once it does.

A workflow change is the standard case. A `repository_dispatch` workflow runs from the default branch's copy of itself, so the pull request's own run exercises the old version and proves nothing about the change.
