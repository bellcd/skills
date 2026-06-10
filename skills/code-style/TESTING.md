# Testing Conventions

## Exercise the real code under test

Always use the actual production code being tested. E.g. when unit testing an HTTP
endpoint, import and use the production code that creates the endpoint — do not
re-create the endpoint inside the test.

## One focus per test

Each test should assert roughly one piece of functionality — the piece its title names.
For UI with many moving parts (e.g. 10 fields arranged in various ways):

- One test can verify everything is present (if it should be).
- Each more specific test asserts only its own concern, not all 10 fields again.

## Expected outputs

Do not use the implementation under test to generate its own expected output. Ideally
the expected outputs are a small enough set of data that each case states them directly:

```ts
expect(formatDuration(90_000)).toBe('1m 30s'); // literal, not formatDuration-derived
```

If expected-output generation must be abstracted to stay manageable, some acceptable options are:

- Fast unit tests for pieces like formatting (e.g. a string).
- Longer/slower integration tests using the logic under test, but only to check things like "wiring" in the DOM.

Avoid mirrored only-for-test helpers that recreate logic to produce expected output.

## Incidental data

Data or choices that the test requires but that aren't part of the logic under test should be randomized or defaulted.

## `.only`

Never remove `.only` from tests. It is added deliberately as a quick way to isolate a
particular test while iterating.

## Playwright

- Page object methods should return `Locator`s, so test cases can use the flakiness-resistant pattern of asserting in the test itself:

  ```ts
  // page object
  get saveButton(): Locator {
    return this.page.getByRole('button', { name: 'Save' });
  }

  // test
  await expect(checkoutPage.saveButton).toBeEnabled();
  ```

- Assert against the DOM with `expect` matchers that check concrete state, e.g. that a particular element has particular text.
- Skip explicit visibility checks — many Playwright methods perform them automatically.
