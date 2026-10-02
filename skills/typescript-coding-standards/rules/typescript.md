# TypeScript contracts

## Function declarations

Use declarations for named utilities, services, factories, hooks, components,
and local helpers. Arrows are appropriate as inline callbacks. A value returned
by a factory or hook is not an arrow declaration to rewrite.

```ts
// Avoid named arrows.
const double = (value: number): number => value * 2;

// Prefer declarations.
function double(value: number): number {
  return value * 2;
}

const doubled = values.map(value => value * 2);
const handler = createMiddleware(async (context, next) => {
  await next();
});
```

Keep contextual typing in callbacks such as request handlers, test mocks, and
React `useCallback`; do not hoist every inline callback into a standalone helper.
Keep generic parameters, overloads, explicit return types, and `async` when
converting named arrows. A typed variable may supply parameter types that must
be transferred to the declaration.

Check lexical `this`/`arguments`, initialization order, and function properties
before a conversion. Preserve lexical binding where it is required; do not apply
unsafe lint fixes blindly. A function-declaration migration must preserve route
contracts and factory inference.

## Function arguments

Use a named argument object for multiple related options, optional values, or
same-typed parameters that are easy to swap. For APIs we own, at most three
positional parameters are allowed; use an object beyond that. A simple unary
utility or a clear two-argument operation does not need a wrapper object.
Preserve framework callbacks and externally defined signatures.

Use the named shapes below and the [comment conventions](comments.md); keep
private helper types private.

```ts
export interface GetEventsArgs {
  /** First block to include. */
  fromBlock: bigint;
  /** Last block to include. */
  toBlock: bigint;
  /** Maximum rows to return. */
  limit: number;
  /** Include events removed by a reorganization. */
  includeRemoved?: boolean;
}

export async function getEvents(args: GetEventsArgs) {
  // Read events using the named options.
}
```

Update callers together when changing a signature. A style task should not
silently break an external API just to satisfy parameter count.

## Object shapes and type inference

Both `interface` and `type` are accepted for object shapes. Follow nearby code;
do not churn one into the other. Use `type` for unions, mapped types, and other
type expressions that interfaces cannot represent directly.

Define function argument objects and component props outside the signature.
Export the named shapes used by exported APIs. Follow the field documentation
conventions in [comments](comments.md).

```ts
export interface BlockRange {
  /** Inclusive first block. */
  fromBlock: bigint;
  /** Inclusive last block. */
  toBlock: bigint;
}

export type SyncStatus = "idle" | "running" | "failed";
```

Preserve useful inference from schemas, libraries, and factories. Do not replace
an inferred schema type with a manually duplicated interface just for style.

## Unknown inputs and type safety

Use `unknown` for untrusted or unknown values, then validate or narrow before
access. Use generics when input/output types are related and infer library types
where possible. Do not hide missing validation behind `as SomeType`, `as any`,
or a double assertion.

```ts
/** Reads a numeric count from an untrusted payload. */
export function parseCount(
  /** Decoded response body, before validation. */
  input: unknown,
): number {
  if (
    typeof input === "object" &&
    input !== null &&
    "count" in input &&
    typeof input.count === "number" &&
    Number.isFinite(input.count)
  ) {
    return input.count;
  }
  throw new Error("Expected a finite count");
}
```

Use the existing runtime schema validator for structured API, environment, or
RPC boundaries. Choose the repo's error type as described in
[runtime error conventions](runtime.md#errors-and-recovery). If an upstream declaration forces an escape
hatch, isolate and explain it at the boundary rather than propagating `any`.

## Literal constants

Use `as const satisfies` for constant maps where literal inference and shape
checking both matter. A broad annotation can erase useful literal types, while
an assertion alone does not check the intended contract.

```ts
const ROUTES = {
  health: "/health",
  events: "/events",
} as const satisfies Record<string, string>;
```

Do not add `as const` to data that is intentionally mutable. Match readonly
arrays in the target type when appropriate; this pattern does not freeze values
at runtime or validate external data.
