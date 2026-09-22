# General

These rules apply to code in any language.

They apply to new code and to code already being edited. In general, older code that predates a rule is not a pattern to copy.
Sweep pre-existing violations in the file(s) being touched. How wide to go with a sweep depends on how much it increases scope, often a user-level decision.
Prefer fixing violations in their own commits. I.e. "make the change easy, then make the easy change"

- Prefer creation functions over shared constants for non-primitive values. A shared object constant is one value aliased everywhere, so a mutation in one consumer leaks to all of them. A creation function hands every caller a fresh value and provides a natural seam for parameters later. Module-level regex constants are exempt, but flag the `lastIndex` hazard if `.test()` or `.exec()` enters the picture.
- Prefer required parameters. Optional and default parameters should be used very sparingly, if at all.
- Factor numeric time constants into conventional units, e.g. `24 * 60 * 60 * 1000`, not `86400 * 1000`.
- Reduce exports where possible. Keep functions, values, etc. module-local unless other modules actually need them.
- Use 0-based indices everywhere and convert to 1-based only at the display site. Name such values `xIndex`. This does not cover ordinal domain scales that merely rank values, which keep their natural numbering.
- Spell names out rather than abbreviating them to opaque acronyms. An acronym is fine when it is itself the canonical public API.
- Give a consumer a narrow interface covering only what it actually uses, rather than a broad shared one with the unused methods stubbed as no-ops.
- Fix a malformed value at its source rather than adding tolerance code downstream. When a script chokes on a config or env value the user owns, correct the value. Reserve normalization for input genuinely outside their control, such as third-party payloads.

## Comments

The default is no comment. Names, file placement, call sites and test names carry the information,
and unlike a comment they stay correct when the code moves.

When a comment seems needed, first try to make it unnecessary.
Rename the variable, extract the function, move the code next to what it relates to.

Then apply the test: delete the comment and reread the code.
If the code still answers the question, it stays deleted.

- A comment that survives says why, not what: a design choice, a constraint the code cannot show, a reason the obvious approach was rejected. These start with `// NOTE:`.
- Don't restate what the code already says, including a function's name, its signature, or its types.
- Don't explain a vendor's documented behavior. What a library or API does by design is in that vendor's docs. The note that survives says why this code answers it the way it does.
- One line. Two where the constraint genuinely needs it. A longer explanation belongs in the README, an ADR, or the tracking issue, with a one-line pointer at the code. A file-level `@fileoverview` comment is the other home for one, where the constraint genuinely spans the file rather than a line in it. It is not a default.
- Don't narrate the change. No "now also handles X", no note of what the code used to do, no summary of what a review pass altered. Git holds that.
- Don't caption steps inside a function. A comment introducing the next few lines is a function waiting to be extracted.
- TODOs in code are a single terse line pointing at deferred work, with a verb matching the actual plan, e.g. `// TODO(#420): add tests`. Rationale belongs in a `NOTE:` comment beside it, not in the TODO. Outside code, in markdown or another prose file with no `NOTE:` convention to hold it, a TODO carries its own rationale rather than shedding it.
- Prefer to break comment lines at clause and phrase boundaries rather than at the wrap width. The prose-style skill's line-breaks section governs this, and covers markdown and other documentation too.

Reread every comment in a diff before handing it over, and delete the ones that fail the test.
