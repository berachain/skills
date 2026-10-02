# Runtime behavior and backend boundaries

## Errors and recovery

Do not catch a failed read and return `0`, `[]`, or another success-shaped value
that conceals the failure. Let errors propagate when the caller already owns
handling. Catch when adding context, mapping an expected domain outcome, or
performing recovery with an explicit contract.

- Use the repository's existing error convention, whether a domain error class,
  native `Error`, framework HTTP errors, or typed outcomes. Preserve established
  reporting behavior and avoid adding a dependency solely to follow this skill.
- Preserve `cause` when wrapping. Keep expected HTTP statuses and distinguish
  missing data from unavailable upstream data.
- Report at the owning boundary using the repository logger/monitoring. Avoid
  duplicate logs at every rethrow and `console` in services that prohibit it.

```ts
try {
  return await loadPrices();
} catch (cause) {
  throw new Error("Failed to load prices", { cause });
}
```

A documented optional-source fallback is allowed when callers can distinguish
degraded results and the failure is handled observably. Do not add retries or
change failure semantics as part of a style-only refactor.

## Services, workers, and runtime libraries

Apply this section to services, workers, indexers, and runtime libraries.

- Keep route handlers and job entrypoints focused on orchestration. Extract
  reusable domain operations into plain functions, not React hooks.
- Pass runtime dependencies through the established context or argument object.
  When context is constructed at startup and passed into operations, preserve
  that pattern rather than introducing module-level client singletons.
- Preserve pure import boundaries. Code intended for codegen or isolated tests
  must not start clients, read validated process environment, or open sockets
  merely by being imported. Use the package's side-effect-free entrypoint when
  one exists.
- Follow package exports and per-app environment restrictions. Import only the
  configuration entrypoint intended for the current runtime; consult package
  instructions before introducing a new dependency across those boundaries.
- Keep schema inference, handler return types, and serialized API output intact
  during style changes. Do not weaken generics to make a conversion compile.

Use the repository's lifecycle, logging, and dependency injection conventions;
this skill does not prescribe a new server architecture.
