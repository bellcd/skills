---
name: code-style
description: Coding conventions. Use when writing, editing, installing, or reviewing code of any kind.
---

# Code Style

Rules phrased "in general" or "prefer" are strong defaults — deviate only for a specific reason and point out why to the user.

Load only the reference files relevant to the current task:

- [GENERAL.md](GENERAL.md) — applies to all languages
- [JAVASCRIPT.md](JAVASCRIPT.md)
- [TYPESCRIPT.md](TYPESCRIPT.md)
- [REACT.md](REACT.md)
- [NPM.md](NPM.md)
- [TESTING.md](TESTING.md)

## Security (always applies)

- Never read unencrypted `.env` files.
- Avoid logging HTTP request/response headers — they often contain secrets.
