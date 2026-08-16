# General

These rules apply to code in any language.

- Prefer creation functions over shared constants for non-primitive values. A shared object constant is one value aliased everywhere — a mutation in one consumer leaks to all of them. A creation function hands every caller a fresh value and provides a natural seam for parameters later. Module-level regex constants are exempt, but flag the `lastIndex` hazard if `.test()` or `.exec()` enters the picture.
- Prefer required parameters. Optional and default parameters should be used very sparingly, if at all.
- Factor numeric time constants into conventional units, e.g. `24 * 60 * 60 * 1000`, not `86400 * 1000`.
- Reduce exports where possible — keep functions, values, etc. module-local unless other modules actually need them.
- Use 0-based indices everywhere and convert to 1-based only at the display site. Name such values `xIndex`. This does not cover ordinal domain scales that merely rank values, which keep their natural numbering.
- Spell names out rather than abbreviating them to opaque acronyms. An acronym is fine when it is itself the canonical public API.
- Give a consumer a narrow interface covering only what it actually uses, rather than a broad shared one with the unused methods stubbed as no-ops.
- Fix a malformed value at its source rather than adding tolerance code downstream. When a script chokes on a config or env value the user owns, correct the value. Reserve normalization for input genuinely outside their control, such as third-party payloads.

## Comments

- Prefer self-documenting variable names over comments.
- Avoid comments that restate what the code already does (e.g. repeating a function's name).
- Comments that explain a design choice or a non-obvious constraint start with `// NOTE:`.
- TODOs are a single terse line pointing at deferred work, e.g. `// TODO(#420): add tests`. Rationale belongs in a `NOTE:` comment, not in the TODO.
- Break comment lines where the sentence naturally pauses, at clause and phrase boundaries. Do this even when that makes lines noticeably shorter or longer than the usual wrap width. Formatters don't reflow comments, so the author's breaks are what the reader gets.
