# Modules and organization

## Exports and imports

Use named exports for functions, types, components, and shared constants. Use
`import type` / `export type` for type-only dependencies, following local tooling.

```ts
export function computeTotal(values: readonly number[]): number {
  return values.reduce((total, value) => total + value, 0);
}
```

Keep default exports when a framework or tool requires them, such as route
components, worker entrypoints, or configuration modules.
Preserve the shape expected by that loader instead of mechanically converting
it. Follow package export boundaries rather than adding deep imports or broad
barrels solely to change export style.

## Feature ownership

Keep helpers, types, tests, hooks, and subcomponents alongside the feature that
owns them. Promote code into a shared package only when multiple features need
the same contract, not merely because a file is growing.

```text
src/pricing/
  loadPrices.ts
  loadPrices.unit.test.ts
  types.ts

src/components/TokenDialog/
  index.tsx
  useTokenDialog.ts
  types.ts
```

These are examples, not a requirement to introduce these directories in every repo.
Follow the current package layout. Keep pure domain calculations independent of
React so services and hooks can share them without importing UI dependencies.
