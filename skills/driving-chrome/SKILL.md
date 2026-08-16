---
name: driving-chrome
description: Two Chrome extension behaviors that abort a batch or blank a result. Use before driving the browser extension, especially before a multi-step batch.
---

# Driving Chrome

Pair first: `list_connected_browsers`, then `select_browser`.

## The first visit to a domain is a standalone navigate

A mid-batch `navigate` to a domain not yet visited fails the permission check, and takes the rest of the batch down with it.

Give each new domain its own `navigate` call, then batch freely once the permission is established.

## `javascript_tool` blanks any result carrying a query string

The entire result is replaced with `[BLOCKED: Cookie/query string data]` when the returned string contains URL query strings. The loss is wholesale, so a single href with `?utm_source=` costs the whole harvest.

Return hostname and pathname only when collecting links.
