---
name: skills-maintenance
description: Use when changing a skill, or after merging a change to the skills repo.
---

# Maintaining the skills

## Edit upstream, never a copy

`bellcd/skills` is the source. Every copy on disk is downstream of it, and editing one in place either loses the edit at the next refresh or diverges invisibly.

Branch the upstream checkout at `~/skills`, change `skills/<name>/`, open a pull request, and let the user merge it. Only then refresh any copy.

## Every change is a release

The repo is a plugin marketplace. Consumers install the plugin `bellcd-skills@bellcd` and see the skills under that prefix, e.g. `bellcd-skills:code-style`.

An install lands in a directory named by version, and `plugin update` reports "already at the latest version" when the version has not moved. So a merged edit that leaves the version alone reaches nobody. Bump it in the same pull request as the skill change.

Two files carry the version and must agree: `.claude-plugin/plugin.json`, and the plugin's entry in `.claude-plugin/marketplace.json`. `validate` checks each manifest's shape. The agreement check is the tag dry run:

```bash
claude plugin validate .
claude plugin tag . --dry-run --force
```

`--force` skips the dirty-tree and tag-already-exists refusals, and a dry run creates nothing, so what remains is the version check. It refuses on a mismatch. Whether to cut the tag after the merge is the release call: a consumer that pins to a tag needs it, one tracking the default branch does not.

A **new** skill also needs its path added to the `skills` array in `plugin.json`, and a line in the README's list. A directory absent from that array is published nowhere, however correct it is.

## Refreshing a local install

After the merge, refresh the marketplace clone, then the plugin:

```bash
claude plugin marketplace update bellcd
claude plugin update bellcd-skills@bellcd
```

`update` defaults to the `user` scope and fails with "not installed at scope user" when the record sits elsewhere. A plugin declared through an organization's managed settings holds a `managed` record and needs `-s managed`. `claude plugin list` shows each record's scope.

Restart Claude Code afterwards. The skill roster is fixed at session start, so a running session keeps the old version whatever the cache now holds.

A cloud session runs whatever its environment was built with. Refreshing that is the consuming environment's business, documented there.

## A single-skill install

The skills CLI installs one skill without the plugin, at `~/.agents/skills/<name>/`, with `~/.claude/skills/<name>` symlinked into it. Refresh it per skill:

```bash
npx skills update <name> -g
```

A **new** skill is not installed yet, so `update` has nothing to update. Install it instead:

```bash
npx skills add bellcd/skills --skill <name> -g
```

Both commands exit successfully, which is what makes this worth knowing. Reading `update`'s "nothing to update" as "already current" leaves the skill uninstalled while looking like a clean refresh. `ls ~/.agents/skills/` is the check that it landed.

`add` installs for every agent target it knows about, and prints a failure line for any target that does not support global installs. Those lines are expected and do not mean the install failed.

## Verifying before you refresh

Check whether a copy is actually stale rather than refreshing on faith. For the plugin, read the installed version from `claude plugin list` and diff against its cache directory:

```bash
diff -rq ~/skills/skills/<name> ~/.claude/plugins/cache/bellcd/bellcd-skills/<version>/skills/<name>
```

For a single-skill install:

```bash
diff -rq ~/skills/skills/<name> ~/.agents/skills/<name>
```

Silence means in sync. A refresh that reports success on an unchanged copy looks identical to one that did real work.
