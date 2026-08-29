---
name: driving-chrome
description: Use before driving a browser. Covers choosing between the Chrome extension and playwright-cli, and the extension's traps.
---

# Driving Chrome

## Two drivers, split by whose session

The Chrome extension drives the real browser, signed in as the user. That is the point of it. Anything already authenticated is its job.

`npx playwright cli` drives a separate browser carrying no session, or one loaded from a saved state file. Reach for it when the user's identity is the thing in the way.

Signed-out checks are the clearest case. The extension carries the user's cookies, so it cannot show what a stranger sees without fighting the profile.

The binary ships inside Playwright. Where a project already depends on Playwright there is nothing to install, and it runs the same engine and browser build the test suite runs. `--help` lists every command, and a fuller skill sits under `playwright-core` in `node_modules` alongside ones for tracing and component testing.

Playwright also bundles an MCP server. It covers the same ground as the extension and adds a third driver to hold in your head. Skip it while the extension is connected.

## A saved state file is a live credential

`state-save` writes session cookies to disk. It belongs in a scratch directory, never in a repo.

## The extension pairs before it drives

Pair first: `list_connected_browsers`, then `select_browser`.

## The first visit to a domain is a standalone navigate

A mid-batch `navigate` to a domain not yet visited fails the permission check, and takes the rest of the batch down with it.

Give each new domain its own `navigate` call, then batch freely once the permission is established.

## `javascript_tool` blanks any result carrying a query string

The entire result is replaced with `[BLOCKED: Cookie/query string data]` when the returned string contains URL query strings. The loss is wholesale, so a single href with `?utm_source=` costs the whole harvest.

Return hostname and pathname only when collecting links.
