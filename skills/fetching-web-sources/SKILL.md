---
name: fetching-web-sources
description: Getting a web source that resists. Use when a fetch is refused or comes back empty, a PDF yields no text, or a page sits behind a login.
---

# Fetching Web Sources

## An attended fetch outreaches the sandbox

`WebFetch` from an ordinary attended session is not behind something like a sandbox egress allowlist. It reaches hosts a sandboxed AFK agent gets refused on.

Try it attended before concluding a source needs a browser.

## A host that refuses every fetch may still open in a browser

Some hosts 403 every automated fetch on every path, including direct file links, and serve the same page to a real browser without complaint. That blocker is genuine and browser-only, so escalate rather than only retrying the fetch.

## A PDF that will not extract is still on disk

Read it with the Read tool as page images, or run `pdftotext -layout`. Between them they handle subsetted CID fonts and scanned documents, which are what defeat ordinary text extraction.

## Google Drive is a standing no

`drive.google.com` is too much of a security risk. No `WebFetch`, browser navigation, `curl`, etc.

Treat a Drive-hosted document as unreachable and ask him for the content directly.

## A logged-out probe of a profile page proves nothing

E.g. Facebook bounces unknown paths to the site root and serves content-unavailable, while LinkedIn throws an authwall. Neither response distinguishes a page that exists from one that does not.

Ask whether a page exists rather than inferring it is dead from only a signed-out browser.

## Confirm a document link resolves to the organization you expect

A link matched on filename alone pointed at an unrelated organization's document for months, because the fetch always 403'd and nobody opened it.
