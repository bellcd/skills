---
name: mobile-push-approvals
description: Use when a push notification reports not sent, mobile approvals look broken, or Christian says he is stepping away.
---

# Mobile Push Approvals

Approval prompts and questions do reach Christian's phone, and he answers them from there. Treat delivery as working.

## "Not sent" is deduplication

`PushNotification` returns *"Not sent — this terminal is active"* whenever he is at the keyboard. That is the dedup rule firing, not a delivery failure and not a registration problem.

## The registration banner is not evidence

The `⚠ No mobile registered` banner in `/config` has shown while push was in fact working. Read it as decoration.

Earlier sessions took a skipped notification and that banner together as proof of a broken device registration, and spent several sessions chasing a server-side handshake that was fine.

## The probe is real work, run while he is away

When he says he is stepping away, fire the work and let a genuine approval prompt or question block on him. That is the only probe that carries information.

## `remoteControlAtStartup` is user scope

The CLI rejects that key from project and local settings files.
