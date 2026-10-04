---
name: prose-style
description: Use when writing or editing prose of any kind.
---

# Prose Style

These are strong defaults. Deviate only for a specific reason, and say why.

## Say it straight

Prefer plain, direct sentences over complicated construction. A sentence stacking subordinate clauses is usually two or three sentences that have not been separated yet.

Avoid semicolons and em dashes. Split into separate sentences instead.

## Short paragraphs

Break dense paragraphs at their natural seams.

When a paragraph packs several distinct ideas, such as what a thing is, then the decision, then the consequence, give each one its own chunk.

## No arbitrary line breaks

One line per paragraph. Never break a line to meet a width.

A hard-wrapped paragraph freezes one person's viewport into the text. Every later edit has to rewrap around it, every diff then shows lines whose substance did not change, and every reader on a different width reads ragged text. Soft wrapping already does the job hard wrapping was invented for.

This holds anywhere prose is written: markdown, code comments, commit messages, pull request descriptions, issue bodies, and replies in a terminal. It holds inside a fenced block carrying prose or a diff, where a break also stops the block being usable as a patch.

The breaks that stay are the ones that mean something: a new paragraph, a list item, a heading.

## References that rot

File links, exact type signatures, and inline dates all go stale as code moves and issues close. A doc peppered with dead pointers reads worse than one naming the concept plainly.

Default to the concept rather than the pointer. E.g. write "the read seam", not a file path.

Keep a pointer where the reader genuinely needs to navigate there, such as the Refs section of an issue that gates on another issue.

When a pointer does earn its place in prose, name what it points at: e.g. `Group-by-goal view (#492)`, never a bare `#492` or different format like `#492 Group-by-goal view`

A date that genuinely needs recording belongs in a structured home, like a revision-history table or a decision log.

## Where the explanation goes

Durable mechanism goes in the README. Follow-up work goes in the tracking issue.

Code and config comments are the last resort, not the first. The code-style skill's Comments section governs them.
