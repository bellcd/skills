# Skills

[Agent Skills](https://agentskills.io) — portable instructions for AI coding agents.

This repo is a Claude Code plugin marketplace.
Installing the plugin is how to get all of these skills at once, in every project, on every machine.

## Installing the plugin

```bash
claude plugin marketplace add bellcd/skills
claude plugin install bellcd-skills@bellcd
```

Skills then resolve under the plugin's name, e.g. `bellcd-skills:code-style`.

An organization can declare the marketplace once for every member instead,
through the Claude Code managed settings.
That declaration reaches cloud sessions as well as local ones.

A marketplace on a network location has to be declared in user or managed settings.
Project scope cannot vouch for one, so a declaration committed to a repo is ignored.

## Installing one skill without the plugin

The [skills CLI](https://github.com/vercel-labs/skills) installs a single skill into one project,
which is worth it when only that skill is wanted:

```bash
npx skills add bellcd/skills --skill code-style
npx skills add bellcd/skills --list
```

Add `-g` for a global install.

Keeping those current is per skill.
`update` takes a **skill name**, not a repo, and it only looks where that skill is actually installed:

```bash
npx skills ls -g                  # check what is installed, and where
npx skills update code-style -g   # global install
npx skills update code-style      # project install, run from the project root
```

## Available skills

- [afk-prompt-files](skills/afk-prompt-files/SKILL.md)
- [ag-grid](skills/ag-grid/SKILL.md)
- [code-style](skills/code-style/SKILL.md)
- [db-normalization](skills/db-normalization/SKILL.md)
- [driving-chrome](skills/driving-chrome/SKILL.md)
- [fetching-web-sources](skills/fetching-web-sources/SKILL.md)
- [git-and-github-hygiene](skills/git-and-github-hygiene/SKILL.md)
- [mobile-push-approvals](skills/mobile-push-approvals/SKILL.md)
- [posthog](skills/posthog/SKILL.md)
- [prose-style](skills/prose-style/SKILL.md)
- [skills-maintenance](skills/skills-maintenance/SKILL.md)
- [tight-feedback-loops](skills/tight-feedback-loops/SKILL.md)

## Releases

An unattended agent should run a reviewed version of a skill,
rather than whatever the default branch happens to serve at that moment.

`claude plugin tag` cuts a version tag for that.
It refuses to tag unless the plugin manifest and the marketplace entry agree on the version:

```bash
claude plugin tag . --dry-run
```

A consumer then pins to the tag.
Both manifests carry the version, so bump them together in the same change.
