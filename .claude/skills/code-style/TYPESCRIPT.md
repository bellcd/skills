# TypeScript

General style rules in [JAVASCRIPT.md](JAVASCRIPT.md) also apply to TypeScript files.

- Never use `as` casts. Create a Zod schema (named with a `z` prefix) and runtime-parse instead.
- Avoid `any`. When other code presents values typed as `any`, Zod-parse them at the boundary layer to get a proper type.
- When a TypeScript type is needed from a Zod schema, infer it on the immediate next line:

  ```ts
  const zUser = z.object({ name: z.string() });
  type User = z.infer<typeof zUser>;
  ```

- Prefer required parameters. Optional and default parameters should be used very sparingly, if at all.
- Only use the `satisfies` keyword when it's actually needed.
