---
name: afk-prompt-files
description: Use when writing or editing an AFK prompt.
---

# AFK Prompt Files

For a ticket-to-PR run, start from [TEMPLATE.md](TEMPLATE.md).

Skills reachable inside the VM do not self-fire. A headless run given only a feature prompt finishes the feature and stops. The review then has to happen attended after the fact.

Whatever the run must do, the prompt says outright, naming each skill as an explicit `/<plugin>:<name>` invocation.

Don't restate what a skill or a repo rule already says. Point at it. Two wordings of one rule get resolved arbitrarily.

A prohibition that must hold belongs in a hook, not the prompt.
