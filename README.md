# Skills

[Agent Skills](https://agentskills.io) — portable instructions for AI coding agents.

## Installing

Install with the [skills CLI](https://github.com/vercel-labs/skills):

```bash
npx skills add bellcd/skills --skill code-style
```

Add `-g` to install globally (available in every project) instead of into the current
project. List everything available here without installing:

```bash
npx skills add bellcd/skills --list
```

## Keeping installs current

`update` takes a **skill name**, and it only looks where the skill is actually installed. A global install needs `-g`:

```bash
npx skills ls -g                  # check what is installed, and where
npx skills update code-style -g   # global install
npx skills update code-style      # project install, run from the project root
```

## Copies that travel

A global install lives in `~/.agents/skills` and is reached through a symlink from `~/.claude/skills`.
It covers every project on that machine, but it does not travel.
An agent working in a sandbox or a fresh clone has a different home directory and never sees it.

A project that needs a skill to reach those agents therefore commits its own copy under `.claude/skills/<skill>/` as ordinary files.
That copy is not CLI-managed — `update` will never touch it — so it drifts. Refreshing it means copying from this repo and reviewing the diff like any other change.

## Available skills

- [code-style](skills/code-style/SKILL.md)
- [git-and-github-hygiene](skills/git-and-github-hygiene/SKILL.md)
- [prose-style](skills/prose-style/SKILL.md)
- [tight-feedback-loops](skills/tight-feedback-loops/SKILL.md)
