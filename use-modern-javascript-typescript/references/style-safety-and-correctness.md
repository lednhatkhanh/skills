# Style, Safety, and Correctness

Examples, config, and patterns that extend the rules in SKILL.md. Do not repeat baseline or rules defined there.

## tsconfig Baseline

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
    "erasableSyntaxOnly": true,
    "isolatedDeclarations": true,
    "target": "esnext",
    "module": "nodenext",
    "skipLibCheck": true
  }
}
```

## Erasable TypeScript

| Banned | Replacement |
|--------|-------------|
| `enum` / `const enum` | Union literals + `as const` objects |
| Runtime `namespace` | Plain modules |
| Parameter properties | Explicit field declarations |
| `import = require()` / `export =` | ESM `import` / `export` |
| Angle-bracket `<Foo>bar` | `as Foo` or narrowing |

Aligns with Node.js native type stripping and `erasableSyntaxOnly: true`.

## Type Modeling Examples

```ts
// type for unions and transforms
export type UserStatus = "active" | "disabled";
export type User = { id: string; email: string; status: UserStatus };

// satisfies: validate without widening
const DEFAULT_CONFIG = { retries: 3, timeout: 5_000 } satisfies Partial<Config>;
```

### Advanced Patterns

```ts
// Constrained/defaulted generics
export type Result<TData, TError = Error> =
  | { ok: true; data: TData }
  | { ok: false; error: TError };

// const type parameters — infer literals
export function createRoute<const TPath extends string>(path: TPath) {
  return { path };
}
const route = createRoute("/users"); // { path: "/users" } not { path: string }

// NoInfer — control inference position
export function createFSM<TState extends string>(
  initial: TState,
  transitions: Record<TState, ReadonlyArray<NoInfer<TState>>>,
) { /* ... */ }

// Conditional + infer
export type AsyncValue<T> = T extends Promise<infer U> ? U : T;

// Mapped + template literal
export type PrefixedKeys<T extends Record<string, unknown>, P extends string> = {
  [K in keyof T as `${P}${Capitalize<string & K>}`]: T[K];
};
```

## Narrowing

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
  if (!isUser(value)) throw new Error("Expected User payload");
}
```

## Modern JavaScript API Examples

### Immutable Arrays

```ts
const sorted = items.toSorted((a, b) => a.rank - b.rank);
const reversed = items.toReversed();
const spliced = items.toSpliced(1, 0, newItem);
const replaced = items.with(2, updatedItem);
```

### Grouping

```ts
const byCategory = Object.groupBy(products, (p) => p.category);
const grouped = Map.groupBy(items, (item) => item.key); // non-string keys
```

### Set Composition

```ts
const a = new Set([1, 2, 3]);
const b = new Set([2, 3, 4]);
a.union(b);               // {1,2,3,4}
a.intersection(b);        // {2,3}
a.difference(b);          // {1}
a.symmetricDifference(b); // {1,4}
a.isSubsetOf(b);          // false
```

### Iterator Helpers

```ts
const results = map.values()
  .filter((v) => v > threshold)
  .map((v) => transform(v))
  .take(10)
  .toArray();
// Also: .drop(), .flatMap(), .reduce(), .some(), .every(), .find()
// Iterator.from(iterable) wraps any iterable
```

### Resource Management

```ts
function processFile(path: string) {
  using handle = openFile(path); // [Symbol.dispose]() on scope exit
  return handle.read();
}
async function processConn() {
  await using conn = await connect(); // [Symbol.asyncDispose]()
  return conn.query("SELECT 1");
}
```

### Other

```ts
const copy = structuredClone(original);                         // deep clone
const { promise, resolve, reject } = Promise.withResolvers<T>(); // deferred
const url = URL.parse(input);                                   // null if invalid
import data from "./config.json" with { type: "json" };         // import attr
```

## Nullability

```ts
// Prefer ?? over ||
const timeoutMs = config.request?.timeoutMs ?? 5_000;

// typeof for runtime checks, both branches for nullish
if (typeof input === "string") { return input.trim(); }
if (typeof entry === "undefined" || entry === null) { return loadFresh(); }
```

## Errors and Results

- Typed `Error` with `cause` chains. Never swallow.
- Discriminated unions for recoverable outcomes.
- Exhaustive `switch` with `never` check.

```ts
function assertNever(x: never): never {
  throw new Error(`Unexpected value: ${x}`);
}
```

## Type-Level Validation

```ts
const status: UserStatus = "active";
// @ts-expect-error invalid status
const invalid: UserStatus = "archived";
```
