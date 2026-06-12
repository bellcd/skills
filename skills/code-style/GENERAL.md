# General

These rules apply to code in any language.

- Prefer creation functions over shared constants for non-primitive values. A shared object constant is one value aliased everywhere — a mutation in one consumer leaks to all of them — while a creation function hands every caller a fresh value and provides a natural seam for parameters later.
- Prefer self-documenting variable names over comments.
- Avoid comments that restate what the code already does (e.g. repeating a function's name).
- Prefer required parameters. Optional and default parameters should be used very sparingly, if at all.
- Factor numeric time constants into conventional units, e.g. `24 * 60 * 60 * 1000`, not `86400 * 1000`.
- Reduce exports where possible — keep functions, values, etc. module-local unless other modules actually need them.
