---
name: artifact-design
description: Use when building or editing an artifact page. Overrides the built-in artifact design defaults.
---

# Artifact Design

These override the built-in artifact design defaults where the two disagree.

## No arbitrary widths

Width comes from the layout grid, not from caps on single elements. No `max-width: 90ch` on paragraphs or lists, no fixed-px meters, no fixed chart label columns.

Text fills its container. Chart label and value columns size to their content, with `max-content` and subgrid so rows align.

This overrides the default of keeping running text near 65 characters. On a wide monitor, a capped paragraph under a full-width chart looks broken.
