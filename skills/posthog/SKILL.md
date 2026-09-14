---
name: posthog
description: Use when working with PostHog.
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

## Triage ends in resolved or suppressed, never active

Error-tracking alerts fire on issue created and issue reopened. An issue that is already active and stays active fires neither, so every later occurrence arrives in silence. The alerting can be configured perfectly and still never mention it.

Resolved is the default outcome. A recurrence reopens the issue, which alerts.

Suppressed is for noise genuinely outside the app's control, and it means no alert from that issue ever again.

Leaving an issue active is the one outcome that looks like triage and is not.

## Suppression is wider than the thing being suppressed

A cross-origin script error reaches `window.onerror` stripped of its message, file, line and stack. The browser does this deliberately. All that survives is the literal string `Script error.`, so the fingerprint has nothing to discriminate on and every such error from every script collapses into one issue.

Suppressing that issue is reasonable while the only third-party tags are ones nobody would act on. It also silently covers every third-party script added afterwards.

So the revisit condition travels with the decision: a new third-party tag whose failures matter means reopening the question.

## Verifying capture needs an async throw

`setTimeout(() => { throw new Error('...') })` reaches `window.onerror`, which the hook autocapture wraps.

A throw typed straight into the DevTools console does not, and records nothing at all. The absence reads as broken alerting rather than as the wrong instrument.

There is no `posthog` global to call when the SDK is initialized as a module import rather than the snippet, so reaching for `posthog.captureException` in a console is a dead end twice over.
