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

## Assert whole structures

Assert against the largest structure that makes sense for what the test is checking, rather than picking off individual properties one at a time. A whole-object assertion also catches unexpected extra or changed fields. There's no need to continually assert the same structure test after test.

```ts
// Prefer
expect(span).toEqual({ sessionIndex: 0, sourceText: expect.stringContaining('given somewhat') });
// Over
expect(span.sessionIndex).toBe(0);
expect(span.sourceText).toContain('given somewhat');
```

Keep two assertions rather than downgrade an exact `toEqual` to a subset match. Exactness on a small type beats one fewer assertion.

`toMatchObject` subset-matches an object, but it pins array length and order. Collapsing a run of per-element assertions into one whole-array `toMatchObject` silently adds ordering coverage.

## No loops in test bodies

Spell each case out as its own assertion line, even when that repeats the matcher and the expected value. A loop hides which case failed. The repetition is the point — each line reads on its own.

This applies to test bodies only. Production code still prefers `for...of` over `.forEach`.

## Assert what your own code controls

Prefer signals your own code decides: the branch taken, a redirect path, literals in a JSON body, a header your code sets. Avoid framework-internal protocol details, which can shift in a version bump and silently invalidate the test.

Before asserting on a value, ask whose code sets it. If the framework sets it, look for an app-controlled formulation of the same outcome. If the behavior is only observable through framework encodings at unit scope, move that pin to an integration or end-to-end seam.

## Incidental data

Data or choices that the test requires but that aren't part of the logic under test should be randomized or defaulted.

## Test data lives beside the assertions

Fixture data that assertions depend on belongs in the test file. A reader should see where every asserted value comes from without opening another file.

Playwright component-test story files exist only because a spec can't define React components inline. Keep them as minimal harnesses holding wrapper state, and pass all data in as serializable props from the spec. A story that owns constants the spec asserts against hides the arrange step.

This extends to data-fetching components. Have them take their query functions as a required prop instead of constructing the source internally. The spec builds that seam from its inline fixtures. Production binds the real one at the page boundary.

Name spec-local helpers without the word "fixture". An unexported helper in a spec file already says it.

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
- When a component renders labeled sections, give its page object one named getter per section. A run of `toContainText` against the panel root passes even when a value renders under the wrong label, and the reader can't map assertion to UI. Derive each locator from the visible label so the label's presence is asserted too, then prefer `toHaveText` (including the array form for lists) now that the locator is that precise.
- A page-object parameter that addresses content takes the user-visible value: `sessionHeading(12)` for the heading reading "Session 12", `card('Shared Attention')`. Name it for what it is. Parameters that address structural position stay 0-based. A page object that re-applies display math derives the expectation through the same transformation the implementation uses, instead of stating it literally.
- When one test legitimately needs longer than the global timeout, give it a single `test.setTimeout(N)` and let the assertion inherit it with `{ timeout: 0 }`. Don't stack a competing per-assertion timeout, and don't raise the global — a tight global is what lets other tests catch real slowness. Add a `// TODO:` to remove the override once the underlying slowness is gone.

## Regex

Prefer exact strings over regex whenever possible, e.g. in matchers.
