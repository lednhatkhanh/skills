# Performance and Lazy Evaluation

Use this reference when performance is relevant or when choosing between eager and deferred execution.

## 1) Optimization Order

1. Preserve correctness.
2. Measure baseline behavior.
3. Optimize bottlenecks only.
4. Re-measure and keep only improvements.

Do not introduce complexity without evidence.

## 2) Type-System Performance

Treat compiler throughput as part of performance for TS-heavy codebases.

- Run diagnostics (`tsc --extendedDiagnostics`) when type-heavy changes significantly affect check time.
- Prefer reusable helper aliases over deeply nested inline conditional types.
- Keep recursive type utilities bounded; do not model unbounded recursion in public types.
- Control unwanted distributive behavior with tuple wrapping (`[T] extends [U]`) when needed.
- Split complex utility types into small named parts to improve maintainability and error readability.

## 3) Lazy Evaluation Patterns

### Generator Pipelines

Use generators to avoid allocating full intermediate arrays when only partial consumption is needed.

```ts
export function* mapLazy<T, R>(
  source: Iterable<T>,
  project: (item: T) => R,
): Generator<R> {
  for (const item of source) {
    yield project(item);
  }
}
```

### Deferred Computation with `??=`

```ts
type Cache = {
  parsedConfig?: ParsedConfig;
};

export function getParsedConfig(cache: Cache, raw: string): ParsedConfig {
  cache.parsedConfig ??= parseConfig(raw);
  return cache.parsedConfig;
}
```

### Lazy Dynamic Imports

Load optional or rare code paths only when needed.

```ts
export async function runHeavyReport(input: ReportInput): Promise<Report> {
  const { buildReport } = await import("./reporting/build-report.js");
  return buildReport(input);
}
```

## 4) Concurrency Choices

- Use `Promise.all` for independent tasks where one failure should fail the group.
- Use `Promise.allSettled` for best-effort fan-out.
- Use bounded concurrency for large batches to avoid resource spikes.

Ordered and dependent side effects should stay sequential.

## 5) Memory and Allocation Hygiene

- Prefer streaming/iterables for large datasets.
- Avoid repeated object spreading in deep loops when it causes excessive allocations.
- Reuse stable instances for expensive regex/parsers where safe.
- Use `toSorted` / `toReversed` when mutation would cause defensive cloning later anyway.

## 6) Common High-Value Improvements

- Exit early on invalid inputs.
- Hoist invariant work outside loops.
- Cache normalized keys for repeated lookups.
- Replace O(n^2) scans with `Map`/`Set` indexing where appropriate.

## 7) Performance Guardrails

- Keep readable code unless measured impact requires lower-level optimization.
- Capture benchmark input distributions close to production behavior.
- Document tradeoffs when introducing caches, throttling, debouncing, or batching.
- Document tradeoffs when introducing advanced type-level utilities that increase type-check cost.
- Do not spend effort on legacy-engine workarounds for runtimes or browsers outside the supported baseline unless explicitly requested.
