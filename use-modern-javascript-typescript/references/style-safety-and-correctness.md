# Style, Safety, and Correctness

Use this reference when implementing or reviewing code structure, typing, nullability, and error semantics.

## 1) Language Baseline

- Prefer ESM modules.
- Keep JavaScript in strict mode semantics.
- Keep TypeScript in strict mode and no-emit type-check by default.

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

If `erasableSyntaxOnly` is unavailable in the installed TS version, keep the same rule manually during review.

## 2) Function-First Design

Rules:

- Use function declarations for named logic.
- Use inline arrow callbacks only when passed directly to APIs (`map`, `filter`, event handlers, etc.).
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

## 3) Prefer `type` Over `interface`

Prefer `type` for object shapes, unions, mapped/conditional types, and aliases:

```ts
export type User = {
  id: string;
  email: string;
  role: "admin" | "member";
};
```

Use `interface` only when its specific behavior is needed (for example declaration merging in external type augmentation).

## 4) Erasable TypeScript Only

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
- `value as Foo` assertions and, when possible, type guards

## 5) Nullability and Defaults

- Use optional chaining for deep access.
- Use nullish coalescing for defaults where `0`, `false`, and `""` are valid values.
- Avoid truthy/falsy defaults for nullable fields.

Prefer:

```ts
const timeoutMs = config.request?.timeoutMs ?? 5_000;
```

Avoid:

```ts
const timeoutMs = config.request && config.request.timeoutMs || 5000;
```

## 6) Errors and Result Semantics

- Throw typed `Error` objects with actionable messages.
- Include cause chains for wrapped errors when available.
- Never swallow errors silently.
- Handle promise rejections at every boundary.
- Use discriminated unions for recoverable domain outcomes.

## 7) Immutability and Shared State

- Treat inputs as immutable unless explicitly documented otherwise.
- Prefer non-mutating collection operations (`map`, `filter`, `reduce`, `toSorted`).
- Avoid shared mutable module-level state unless unavoidable and synchronized.

## 8) Async Control Flow

- Use `for...of` with `await` for ordered side effects.
- Use `Promise.all` only when operations are independent and fail-fast behavior is desired.
- Use `Promise.allSettled` when partial success is acceptable.
- Do not use `forEach(async ...)` for sequencing or completion tracking.

## 9) Consistency Checklist

- Names: stable domain terms, no ambiguous abbreviations.
- Types: explicit at public boundaries; inference inside small local scopes.
- Exports: avoid default exports in shared libraries unless project convention requires them.
- Files: one cohesive responsibility per module.
