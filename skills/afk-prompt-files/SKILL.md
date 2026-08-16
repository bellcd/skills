---
name: afk-prompt-files
description: Use when writing or editing an AFK prompt.
---

# AFK Prompt Files

<!-- TODO: add a prompt template, behind a context pointer so only prompt-authoring loads it.
     Compose the battle-tested skills by reference rather than restating them: test-first,
     the two-axis review, CI triage, draft-PR conventions. Each has to appear as an explicit
     /<plugin>:<name> in the prompt body, since plugin commands are user-invocation-only and
     a headless run cannot reach them any other way. -->

Skills reachable inside the VM do not self-fire. A headless run given only a feature prompt finishes the feature and stops. The review then has to happen attended after the fact.

Whatever the run must do, the prompt says outright.

## Every prompt carries a Review section

Place `## Review` before `## Finish`: run the two-axis code-review skill against the branch diff with main, and apply the findings before opening the draft PR.

Keep the test-first instruction in the prompt as well.

The PR body carries no recap of the review findings.

A mechanism that would make these standing rules automatic is still undecided, so per-prompt instruction is the only lever.
