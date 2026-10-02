# Test behavior, not the edit

## Choose an observable contract

Add coverage for new behavior and bug fixes. Given an input or interaction,
assert what the code keeps, drops, orders, returns, throws, or changes externally.
Choose expected outcomes from the requirement or domain contract, independently
of the implementation under test.

Before adding a test, identify the incorrect behavior it would catch. If its
only purpose is to confirm that the edit you just made exists, skip it. Static
data, documentation, formatting, and mechanical style changes do not need new
tests merely to demonstrate that work was done; run relevant existing checks.

## Do not write self-assessing tests

Do not add tests that merely:

- Search source text for a function name, comment, import, or syntax you added.
- Confirm a literal you just inserted appears in a constant array, config,
  allowlist, rendered string, or snapshot with no meaningful behavior exercised.
- Reimplement the production algorithm to compute its own expected result.
- Mock the operation being tested, then assert that the mock returns its setup.
- Assert internal structure or helper calls solely to match the chosen refactor.

For example, adding a URL to a constant allowlist does not justify this test:

```ts
expect(allowedOrigins).toContain("https://example.com");
expect(source).toContain("NEW_ORIGIN");
```

Test a decision instead, when there is one:

```ts
expect(selectActiveUsers([
  { id: "a", active: false },
  { id: "b", active: true },
  { id: "c", active: true },
])).toEqual([
  { id: "b", active: true },
  { id: "c", active: true },
]);

expect(() => clamp({ value: 5, min: 10, max: 0 })).toThrow(RangeError);
```

Literal expectations, snapshots, and mock-call assertions can be useful when
they verify a real output or interaction contract, such as a serialized response
or delivery to an external dependency. Exercise the real code that owns that
decision. Do not ban these assertion styles or add an artificial abstraction
just to make a static edit testable.

## Select the appropriate test boundary

- **Unit:** isolated and deterministic, with no real network or database access.
- **Integration:** verifies boundaries using controlled dependencies, such as
  a temporary database or local service.
- **E2E:** verifies observable behavior through the application interface.
- **Debug:** manual diagnostics that may use real services; excluded from default CI.

Mock external dependencies when appropriate, keeping the behavior under test
real. For React, test visible behavior and interactions. For backend changes,
cover response contracts, error handling, and externally meaningful side effects.

Read the repository's test configuration before choosing filenames or commands.
Some suites select suffixed files such as `*.unit.test.ts`; others use directories
or `*.test.ts`. Use configured scripts and selectors, confirm tests were actually
discovered, and report prerequisites that prevented a tier from running.

For regressions, demonstrate the test fails for the original bug when practical.
Do not weaken assertions or regenerate expected outputs merely to get a passing
result; first establish whether the contract intentionally changed.
