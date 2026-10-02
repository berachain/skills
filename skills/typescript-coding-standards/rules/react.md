# React and Next.js

## Component organization

For components organized in folders, use a PascalCase folder with `index.tsx` as
the main file and a matching named component. Descriptive nested paths can
compose names: `Card/Address/index.tsx` exports `CardAddress`.

```text
TokenDialog/
  index.tsx
  useTokenDialog.ts
  types.ts
```

Keep component-specific hooks and helpers directly in the component folder.
Add subdirectories only when several related files benefit from their own
grouping; a single hook does not need a `hooks/` directory.

Follow an existing flat component layout where the consuming repo explicitly
uses one. Do not reshape backend modules into component folders.

Keep props in a named external shape and document the fields. Components
primarily compose hooks and render their output; JSX-specific branches and
presentation remain in the component. Define child components at module scope
instead of creating a new component identity inside each render.

Use stable domain identifiers for list keys when items can move or change.
Keep render pure; perform event-driven behavior through handlers and external
synchronization through the feature hook.

## Callback naming

For React callback props, use `onSelect`, `onClose`, or another event name.
Use `handleSelect` for a local handler that adds behavior before calling the
prop. Pass a prop through directly when no extra behavior is needed.

```tsx
export interface CloseButtonProps {
  /** Called when the user requests dismissal. */
  onClose: () => void;
}

export function CloseButton({ onClose }: CloseButtonProps) {
  return <button onClick={onClose}>Close</button>;
}
```

Put feature behavior in the existing feature hook. This naming convention does
not require renaming backend framework methods, event APIs, or domain operations
such as `loadPrices` to resemble React props.

## Feature hooks

Keep feature state, effects, queries, derived state, and behavioral handlers in
a colocated hook such as `useTokenDialog`. Extend an existing feature hook rather
than starting a second behavior layer in the component. JSX presentation and
simple rendering branches can remain in the component.

Extract pure calculations into ordinary functions that a hook can call. Do not
turn every helper into a hook or introduce React into shared backend code.

- Call hooks such as `useState` and `useEffect` at the top level of components
  or custom hooks, before conditional returns; do not put them inside event
  handlers, loops, or ordinary service functions.
- Derive values from current inputs instead of mirroring them in state through
  effects. Use effects for synchronization with external systems, including
  appropriate cleanup for subscriptions and async work.
- Keep dependency lists accurate. Do not suppress dependency lint errors to
  freeze a stale closure. Use the repo's existing query/cache layer for fetching.
- Use `useMemo` and `useCallback` when computation cost or identity matters, not
  as automatic wrappers. Their inline arrow callbacks are valid under
  [function declaration conventions](typescript.md#function-declarations).

Respect the installed React version's APIs and special cases; see the official
[Rules of Hooks](https://react.dev/reference/rules/rules-of-hooks).

## Next.js apps

Prefer Server Components for data loading and noninteractive composition. Keep
client boundaries small; use Client Components where hooks, events, or browser
APIs require them. Keep route segments on the server when exporting server-only
metadata or configuration and compose client children below them.

Preserve framework filenames and required default component exports. These are
exceptions to ordinary module naming and named-export conventions.

Inspect the installed Next.js version and caching configuration before changing
revalidation. Choose freshness deliberately for external reads; include relevant
environment and query inputs in cache identity and do not place user-specific
data in a shared cache accidentally. Preserve the repo's existing caching
abstraction.

Do not require `revalidate` on every async segment or wrap every external read in
`unstable_cache`. Those choices depend on runtime and framework configuration;
newer Next.js setups may use Cache Components and `use cache`. Do not migrate
the caching model as a side effect of applying coding standards.

Consult the official [server/client guidance](https://nextjs.org/docs/app/getting-started/server-and-client-components)
and [unstable_cache reference](https://nextjs.org/docs/app/api-reference/functions/unstable_cache)
for the version in use.
