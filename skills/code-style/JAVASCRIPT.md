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
