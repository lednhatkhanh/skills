# Pitfalls and Anti-Patterns

Anti-pattern → fix lookup. Rules from SKILL.md are not repeated here.

## Async

| Anti-pattern | Fix |
|---|---|
| `forEach(async ...)` expecting sequencing | `for...of` + `await`, or `Promise.all(arr.map(...))` |
| `Promise.all` when partial success OK | `Promise.allSettled` |
| Floating promises | Always `await`, return, or `.catch` |
| `new Promise((res, rej) => ...)` wrapping | `Promise.withResolvers()` |

## Mutation

| Anti-pattern | Fix |
|---|---|
| `.sort()` on shared array | `toSorted()` |
| `.reverse()` on shared array | `toReversed()` |
| `.splice()` on shared array | `toSpliced()` |
| `arr[i] = x` on shared array | `.with(i, x)` |
| `{ ...obj }` assuming deep copy | `structuredClone()` |
| Mutating utility function inputs | Return new values |

## Types and Nullability

| Anti-pattern | Fix |
|---|---|
| `==` / `!=` | `===` / `!==` |
| `\|\|` for nullable defaults | `??` (protects `0`, `false`, `""`) |
| `as SomeType` without validation | Type guards, schema validation, `unknown` boundaries |
| Broad `any` | `unknown` + narrow early |
| One-sided nullish check | `typeof` for runtime type; both `undefined` and `null` for nullish; `!!` only for truthiness |
| Missing `satisfies` | `const x = { ... } satisfies Type` |
| `enum` / runtime `namespace` | Union literals + `as const`, plain modules |

## Advanced Types

| Anti-pattern | Fix |
|---|---|
| Unconstrained generics `<T>` | Add constraints/defaults `<T extends ...>` |
| Deep recursive conditional/mapped types | Bound recursion, split named helpers |
| Unintended distributive conditionals | `[T] extends [U] ? X : Y` |
| `&` intersections for object shapes | `interface` (cached, checked once) |
| Missing return types on exports | Add explicit return type annotations |

## Numeric and Date

| Anti-pattern | Fix |
|---|---|
| `a === b` on float results | `Math.abs(a - b) < Number.EPSILON * scale` |
| Mixing local/UTC implicitly | Normalize per boundary (`Date`, `Intl`, temporal lib) |

## Control Flow

| Anti-pattern | Fix |
|---|---|
| Non-exhaustive discriminated union | Exhaustive `switch` + `never` check |
| `?.` where missing data is fatal | `throw` / assertion function at boundary |
| Class for stateless logic | Functions + composition |
| Manual resource cleanup | `using` / `await using` |
| Legacy API when modern exists | `toSorted` not `.sort()`, `Object.groupBy` not reduce, `??` not `\|\|`, iterator helpers not intermediate arrays |
