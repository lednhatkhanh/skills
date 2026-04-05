# Performance and Lazy Evaluation

Optimization, lazy execution, concurrency, and compiler performance. Rules from SKILL.md are not repeated here.

## Optimization Order

1. Preserve correctness. 2. Measure baseline. 3. Optimize bottlenecks only. 4. Re-measure. No complexity without evidence.

## Compiler Performance

- `tsc --extendedDiagnostics` for type-heavy changes.
- `interface` over `&` intersections for object shapes — interfaces cache; intersections re-evaluate.
- Explicit return types on exported functions reduce inference work.
- Named type aliases over deeply nested inline conditionals.
- Bounded recursive types. `[T] extends [U]` to control distribution.
- `skipLibCheck: true`, `isolatedDeclarations: true`.
- Avoid barrel files in large projects.

## Lazy Patterns

### Iterator Helpers (preferred)

```ts
const top = products.values()
  .filter((p) => p.price > 100)
  .map((p) => p.name)
  .take(5)
  .toArray();
```

Prefer over generators for standard transform/filter/take. Use generators only for custom yield logic.

### Generators

```ts
function* mapLazy<T, R>(source: Iterable<T>, fn: (item: T) => R): Generator<R> {
  for (const item of source) yield fn(item);
}
```

### Deferred Computation

```ts
cache.parsedConfig ??= parseConfig(raw);                              // ??= lazy init
const { buildReport } = await import("./reporting/build-report.js");  // dynamic import
const { promise, resolve, reject } = Promise.withResolvers<T>();      // deferred promise
```

## Concurrency

| Pattern | When |
|---------|------|
| `Promise.all` | Independent tasks, fail-fast |
| `Promise.allSettled` | Best-effort, partial success OK |
| Bounded concurrency | Large batches, avoid resource spikes |
| Sequential `for...of` + `await` | Ordered dependent side effects |

## Memory

- Iterator helpers / streaming for large datasets — no intermediate arrays.
- `structuredClone()` for deep copies. Avoid repeated spreading in loops.
- `toSorted()`/`toReversed()`/`toSpliced()`/`.with()` — immutable, allocation-efficient.
- Reuse stable `RegExp` / parser instances.
- `using` / `await using` for deterministic cleanup.

## Quick Wins

- Exit early on invalid inputs.
- Hoist invariants outside loops.
- `Map`/`Set` indexing over O(n²) scans.
- `Set` composition over manual iteration.
- `Object.groupBy()` over manual reduce.
- Cache normalized keys for repeated lookups.
