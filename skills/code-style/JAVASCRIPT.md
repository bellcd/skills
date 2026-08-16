# JavaScript

These rules apply to both JavaScript and TypeScript files.

- Single quotes in JS/TS files.
- Always use native ES modules, never CommonJS.
- Prefer `??` over `||`
- Prefer if-else and similar flow-control statements when possible.
- Use object shorthand: `{ foo }`, not `{ foo: foo }`.
- The `await` keyword goes on its own line, not inline in expressions.
- Imports go at the top of the file. Only `@fileoverview` comments may come before them.
- Dependencies come first in source order.
- Name boolean-returning pure functions `isFoo`.
- When a function takes an object of named keys, name that parameter `params`. Avoid destructuring in the function signature.
- Never take a bare boolean parameter whose meaning is invisible at the call site. `resolveNavHref(user !== undefined)` says nothing about what the boolean selects — either rename the function so the argument's meaning is obvious, or take a `params` object with named properties: `resolveNavHref({ isKnownSignedIn: user !== undefined })`. The same reasoning applies to enums and magic numbers passed positionally.
- Prefer `for...of` loops over `.forEach()`, using `for (const [index, item] of items.entries())` when the index is needed. Plain loops support early exit and read as statements rather than callbacks.
- Take a trailing subarray with an explicit non-negative start index: `items.slice(items.length - n)`, never `items.slice(-n)`. The explicit form states the start position instead of making the reader compute it.
