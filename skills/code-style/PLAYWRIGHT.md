# Playwright

General testing rules in [TESTING.md](TESTING.md) also apply to Playwright specs.

## Page objects return locators

Page object methods should return `Locator`s, so test cases can use the flakiness-resistant pattern of asserting in the test itself:

```ts
// page object
get saveButton(): Locator {
  return this.page.getByRole('button', { name: 'Save' });
}

// test
await expect(checkoutPage.saveButton).toBeEnabled();
```

- Assert against the DOM with `expect` matchers that check concrete state, e.g. that a particular element has particular text.
- Skip explicit visibility checks. Many Playwright methods perform them automatically.
- When a component renders labeled sections, give its page object one named getter per section. Derive each locator from the visible label so the label's presence is asserted too, then prefer `toHaveText` (including the array form for lists).
- A page-object parameter that addresses content takes the user-visible value, e.g. `card('Shared Attention')`. Name it for what it is.

## One tunable timeout per test

When one test legitimately needs longer than the global timeout, give it a single `test.setTimeout(N)`. Don't stack competing per-assertion timeout(s), and don't raise the global timeout.

An assertion that would otherwise impose its own takes `{ timeout: 0 }` so it inherits the test's number rather than competing with it, leaving one tunable number per test.

Add a `// TODO:` or `// NOTE:` to fix or explain the underlying slowness and the override.

## Component tests

Story files exist only because a spec can't define React components inline. Keep them as minimal harnesses holding wrapper state, and pass all data as serializable props from the spec.
