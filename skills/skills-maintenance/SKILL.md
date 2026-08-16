---
name: skills-maintenance
description: Use when changing a skill, or after merging a change to the skills repo.
---

# Maintaining the skills

## Edit upstream, never the copy

`bellcd/skills` is the source. Every other copy on disk is downstream of it, and editing one in place either loses the edit at the next refresh or diverges invisibly.

Branch the upstream checkout at `~/skills`, change `skills/<name>/`, open a pull request, and let Christian merge it. Only then refresh the copies.

Two kinds of copy exist and they refresh differently.

## The machine-wide copy

`~/.agents/skills/<name>/` holds it, and `~/.claude/skills/<name>` is a symlink into that directory. Refresh it with the skills CLI:

```bash
npx skills update <name> -g
```

Run it once per changed skill. It reads the source repo from the skill itself, so no lockfile is involved and no path argument is needed. It reports what it updated, and reports finding nothing when the copy is already current.

## A repo's vendored copy

A repo that vendors skills, so its agents work without a home directory or network, holds them in its own directory and refreshes by plain overwrite from the upstream checkout:

```bash
rsync -a --delete ~/skills/skills/<name>/ <repo>/<vendored-dir>/<name>/
```

`--delete` matters. Without it a file removed upstream survives in the copy forever.

Then diff the two directories to prove the copy is byte-identical before committing. A vendor-refresh commit whose diff does not match upstream exactly is the one thing this layout exists to prevent.

## Verifying before you refresh

Check whether a copy is actually stale rather than refreshing on faith:

```bash
diff -rq ~/skills/skills/<name> ~/.agents/skills/<name>
```

Silence means in sync. This is worth running first, because a refresh that reports success on an unchanged copy looks identical to one that did real work.
