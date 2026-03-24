# Style, Safety, and Correctness

Use this reference when implementing or reviewing code structure, typing strategy, nullability, and runtime/error semantics.

## Table of Contents

- 1) Language Baseline
- 2) Function-First Design
- 3) Type Modeling (`type` vs `interface`)
- 4) Advanced Type Patterns
- 5) Narrowing and Assertion Boundaries
- 6) Erasable TypeScript Only
- 7) Nullability and Defaults
- 8) Errors and Result Semantics
- 9) Immutability and Shared State
- 10) Async Control Flow
- 11) Type-Level Validation
- 12) Consistency Checklist

## 1) Language Baseline

- Prefer ESM modules.
- Default to Node.js 24+, TypeScript 5.9+, and evergreen browsers Chrome 146+, Firefox 148+, and Safari 26+.
- If React or Next.js are relevant, assume React 19+ and Next.js 16+ App Router only.
- Keep JavaScript in strict mode semantics.
- Keep TypeScript in strict mode and no-emit type-check by default.
- Prefer strict equality (`===`, `!==`) over loose equality (`==`, `!=`).

Recommended `tsconfig` baseline:

```json
{
  "compilerOptions": {
    "strict": true,
    "alwaysStrict": true,
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,
    "useUnknownInCatchVariables": true,
    "noFallthroughCasesInSwitch": true,
    "noImplicitOverride": true,
    "verbatimModuleSyntax": true,
    "erasableSyntaxOnly": true
  }
}
```

## 2) Function-First Design

- Use function declarations for named logic.
- Use inline arrow callbacks only when passed directly to APIs (`map`, `filter`, event handlers).
- Prefer pure functions and explicit inputs/outputs.
- Keep functions small and behavior-focused.

Prefer:

```ts
export function normalizeEmail(email: string): string {
  return email.trim().toLowerCase();
}
```

Avoid:

```ts
export const normalizeEmail = function (email: string): string {
  return email.trim().toLowerCase();
};
```

## 3) Type Modeling (`type` vs `interface`)

- Prefer `type` for unions, mapped/conditional/template-literal transforms, and alias composition.
- Use `interface` for extendable object contracts and declaration-merging use cases.
- Keep public API shapes readable before adding type-level abstractions.

Prefer:

```ts
export type UserStatus = "active" | "disabled";

export type User = {
  id: string;
  email: string;
  status: UserStatus;
};
```

## 4) Advanced Type Patterns

Use advanced types for API clarity, not novelty.

### Constrained and Defaulted Generics

```ts
export function pick<TObj extends object, TKey extends keyof TObj>(
  obj: TObj,
  key: TKey,
): TObj[TKey] {
  return obj[key];
}

export type Result<TData, TError = Error> =
  | { ok: true; data: TData }
  | { ok: false; error: TError };
```

### Conditional Types and `infer`

```ts
export type AsyncValue<T> = T extends Promise<infer U> ? U : T;
```

### Mapped and Template Literal Types

```ts
export type PrefixedKeys<
  T extends Record<string, unknown>,
  TPrefix extends string,
> = {
  [K in keyof T as `${TPrefix}${Capitalize<string & K>}`]: T[K];
};
```

## 5) Narrowing and Assertion Boundaries

- Narrow `unknown` at runtime boundaries.
- Prefer type guards and assertion functions over broad assertions.
- Keep assertions small and close to parsing/IO boundaries.
- Do not use one-sided nullish guards. Use `typeof value === "..."` when the runtime type matters, handle both `undefined` and `null` when checking explicit nullish presence/absence, and use `!!value` only when truthiness is the actual condition.

Prefer:

```ts
export function isUser(value: unknown): value is User {
  return (
    typeof value !== "undefined" &&
    value !== null &&
    typeof value === "object" &&
    "id" in value &&
    "email" in value
  );
}

export function assertUser(value: unknown): asserts value is User {
  if (!isUser(value)) {
    throw new Error("Expected User payload");
  }
}
```

## 6) Erasable TypeScript Only

Disallow constructs that require non-trivial runtime transforms:

- `enum` / `const enum`
- runtime `namespace`
- parameter properties (`constructor(private x: X)`)
- `import = require(...)` and `export =`
- angle-bracket assertions (`<Foo>bar`)

Prefer erasable alternatives:

- union literals + `as const` objects instead of `enum`
- plain modules instead of namespaces
- explicit field declarations and assignment in function bodies
- `import` / `export` ESM syntax
- narrowing and assertion functions over broad casts

## 7) Nullability and Defaults

- Use `===` and `!==` for equality checks; do not use `==` or `!=`.
- Do not use one-sided nullish checks when the intent is "present" or "absent".
- Use `typeof value === "..."` when code depends on a specific runtime type.
- When checking explicit nullish absence, use `typeof value === "undefined" || value === null`.
- When checking explicit nullish presence, use `typeof value !== "undefined" && value !== null`.
- Use `!!value` only when checking truthiness is sufficient and `0`, `false`, and `""` are not meaningful values.
- Use optional chaining for deep access.
- Use nullish coalescing for defaults where `0`, `false`, and `""` are valid values.
- Avoid `||` defaults for nullable fields.

Prefer:

```ts
const timeoutMs = config.request?.timeoutMs ?? 5_000;
```

Prefer:

```ts
if (typeof input === "string") {
  return input.trim();
}

if (typeof cacheEntry === "undefined" || cacheEntry === null) {
  return loadFreshValue();
}

if (!!isReady) {
  startWork();
}
```

Avoid:

```ts
const timeoutMs = config.request && config.request.timeoutMs || 5000;
```

Avoid:

```ts
if (status == "ready") {
  startWork();
}
```

## 8) Errors and Result Semantics

- Throw typed `Error` objects with actionable messages.
- Include cause chains for wrapped errors when available.
- Never swallow errors silently.
- Handle promise rejections at every boundary.
- Use discriminated unions for recoverable domain outcomes.
- Enforce exhaustive `switch` handling with `never` checks.

## 9) Immutability and Shared State

- Treat inputs as immutable unless explicitly documented otherwise.
- Prefer non-mutating collection operations (`map`, `filter`, `reduce`, `toSorted`).
- Avoid shared mutable module-level state unless unavoidable and synchronized.
- Do not add polyfills or compatibility workarounds for unsupported legacy runtimes as part of normal guidance.

## 10) Async Control Flow

- Use `for...of` with `await` for ordered side effects.
- Use `Promise.all` only when operations are independent and fail-fast behavior is desired.
- Use `Promise.allSettled` when partial success is acceptable.
- Do not use `forEach(async ...)` for sequencing or completion tracking.

## 11) Type-Level Validation

- Add compile-time tests for non-trivial utility types and generic contracts.
- Cover both valid inference and invalid calls.

Example:

```ts
const status: UserStatus = "active";

// @ts-expect-error invalid status should fail
const invalidStatus: UserStatus = "archived";
```

## 12) Consistency Checklist

- Names: stable domain terms, no ambiguous abbreviations.
- Types: explicit at public boundaries; inference inside small local scopes.
- Exports: avoid default exports in shared libraries unless project convention requires them.
- Files: one cohesive responsibility per module.
- Compatibility: no legacy-runtime branches unless explicitly required by the task.
