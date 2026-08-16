# TypeScript

General style rules in [JAVASCRIPT.md](JAVASCRIPT.md) also apply to TypeScript files.

- Never use `as` casts. Create a Zod schema (named with a `z` prefix) and runtime-parse instead.
- Avoid `any`. When other code presents values typed as `any`, Zod-parse them at the boundary layer to get a proper type.
- When a TypeScript type is needed from a Zod schema, infer it on the immediate next line:

  ```ts
  const zUser = z.object({ name: z.string() });
  type User = z.infer<typeof zUser>;
  ```

- Only use the `satisfies` keyword when it's actually needed.
- Avoid `null` for app-internal absence — use `undefined`, e.g. `useState<string | undefined>(undefined)` and optional-chain results left bare rather than coerced with `?? null`. Reserve `null` for boundaries to systems you don't control: React render returns, DOM and framework API types, external payloads. Mixing both idioms internally forces `?? null` ceremony at call sites.
- Array types: `T[]` is fine for a simple element type (`string[]`, `User[]`). When the element type is more complicated — an inline object literal, a union, a function type — use the `Array<T>` form so the brackets don't trail off the end of a long line:

  ```ts
  // Prefer
  targets: Array<{ value: string; success: string; cueLevel: string }>;
  // Over
  targets: {
    value: string;
    success: string;
    cueLevel: string;
  }
  [];
  ```
