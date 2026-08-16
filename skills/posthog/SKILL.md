---
name: posthog
description: Conventions for PostHog session recording and replay masking. Use when working with PostHog.
---

# PostHog

## Start and stop session recording in the same place

E.g. `posthog.startSessionRecording()` can go in a `useEffect` body and `posthog.stopSessionRecording()` in that same effect's cleanup return.

Stopping on React unmount is fine. What isn't fine is splitting start and stop across files or across lifecycle events, which hides the recording's lifetime from anyone reading either half.

## Masking follows the DOM tree, so portals escape it

`ph-no-capture` masks a subtree. Anything rendered through a portal leaves that subtree, so it renders outside the mask and lands in the replay unmasked, even though the JSX looks nested inside a masked wrapper.

This is the failure mode to watch for with any component library whose overlays portal to the document body. Dialogs, popovers, tooltips, and dropdown menus are the usual suspects.

Two ways out, and they compose:

- Have the component re-declare `ph-no-capture` on its own root, so the masking travels with the component instead of depending on an ancestor.
- Give portal-rendering primitives an explicit opt-in prop, and apply the class to both the overlay and the content, since they portal separately.

A wrapper added only for masking should not disturb layout. `display: contents` on it keeps the grid or flex parent talking to the real children.

## Regression guard

Masking is invisible in the rendered page, so a component test can assert the class is present but not that the recording honored it. The class assertion is worth having as a regression guard. It is not the verification.
