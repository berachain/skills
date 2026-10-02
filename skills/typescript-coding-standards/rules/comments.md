# Comments and JSDoc

## Describe the function's contract

Use JSDoc (`/** ... */`) for function documentation. Describe what the function
does for its caller: its result, meaningful input constraints, side effects, and
failure behavior. Keep it concise and accurate; do not just repeat its name or
narrate the implementation line by line.

JSDoc is not a decision log. Do not record refactoring history, rejected
approaches, review discussions, or explanations of why the author made an edit.
Document the current contract, not how the code arrived there.

Never use `@param` tags. Document positional parameters inline and object fields
on their declarations. Document each field of public argument and result shapes,
including units and optional behavior where relevant. Use `@default` only for an
actual default; avoid duplicating information already clear from the types.

```ts
export interface ClampArgs {
  /** Value to constrain. */
  value: number;
  /** Inclusive lower bound. Must not exceed max. */
  min: number;
  /** Inclusive upper bound. */
  max: number;
}

/** Constrains a value to an inclusive range; throws if the bounds are reversed. */
export function clamp({ value, min, max }: ClampArgs): number {
  if (min > max) {
    throw new RangeError("min must not exceed max");
  }
  return Math.min(max, Math.max(min, value));
}
```

For positional parameters, put a short JSDoc comment directly above the
parameter when its meaning needs explaining. Keep private helper types private;
export the structured types used by public APIs.

## Explain non-obvious logic where it occurs

An implementation rationale belongs inside the function, beside the relevant
logic, only when it helps a future reader preserve an invariant, understand an
external constraint, or avoid reintroducing a subtle bug. Ordinary inline
comments are appropriate here; reserve declaration JSDoc for the contract.

```ts
// Clear the cached promise so a failed attempt does not block future retries.
pendingRequest = undefined;
```

Explain the enduring reason, not a chronological decision log such as "changed
this after review" or "the previous implementation used a different approach."
If the code is self-explanatory and there is no useful constraint to capture,
omit the comment. Update or remove comments when the behavior changes.
