---
name: git-and-github-hygiene
description: Conventions for branching, committing, pushing, pull requests, and GitHub issues. Use when committing, opening or updating a PR, or creating, editing, or closing an issue.
---

# Git and GitHub Hygiene

Load only the reference file relevant to the current task:

- [GIT.md](GIT.md) — branches, commits, pushes
- [PULL-REQUESTS.md](PULL-REQUESTS.md)
- [ISSUES.md](ISSUES.md)

## Never use the user's name in GitHub artifacts

Issues, pull requests, comments, and commit messages use role phrasing — "dev verified locally".
Where a tag is genuinely needed, use the user's handle, e.g. `@bellcd`.

## `gh pr edit` and `gh issue edit` break on a linked Projects board

In a repo with a GitHub Projects v2 board linked, both commands can fail on a `projectCards` GraphQL deprecation. They fail before touching the body, so the error says nothing about the edit you were making.

Fall back to the REST endpoint, which never goes near the project fields:

```bash
gh api repos/{owner}/{repo}/pulls/<n> -X PATCH -f body="$(cat new.md)"
gh api repos/{owner}/{repo}/issues/<n> -X PATCH -f body="$(cat new.md)"
```
