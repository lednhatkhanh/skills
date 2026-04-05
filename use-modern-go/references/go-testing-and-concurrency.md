# Go Testing and Concurrency

Testing strategy, concurrency safety, and reliability patterns. Rules and validation gates from SKILL.md are not repeated here.

## Testing Strategy

- Fast deterministic unit tests. Integration tests at transport/storage boundaries. E2E for critical business flows.
- Actionable failures with inputs and expected/actual context.

## Unit Tests

- Name by behavior: `TestParseRejectsInvalidHeader`.
- Arrange-act-assert. `t.Helper()` in shared helpers.
- Use `t.Context()` for test-scoped cancellation (Go 1.24+).
- Use `testing/synctest` for deterministic concurrent tests (Go 1.25+).
- One failure reason per assertion block. No shared mutable globals.

## Table-Driven Tests

- Table-driven for repeated logic patterns. Stable concise subtest names.
- Capture loop variables correctly in closures/goroutines.
- Include key values in failure messages.

## Assertions

- Plain Go comparisons for simple checks.
- `cmp.Diff` for complex values.
- `errors.Is`/`errors.As` for errors. Avoid brittle string matching.

## Integration Tests

- Real transports with test servers when feasible.
- Verify serialization, retries, deadlines, idempotency.
- Isolated reproducible fixtures. `testdata/` for stable files.

## Fuzzing

- Fuzz parser-like code and untrusted input boundaries.
- Seed corpus with known edge and regression cases.
- Promote crashes into deterministic regression tests.

## Benchmarking

- Only hot paths and allocation-sensitive code.
- Use `b.Loop()` for benchmark loops (Go 1.24+) — cleaner than manual `for i := 0; i < b.N; i++`.
- `b.ReportAllocs()` when allocation behavior matters.
- Consume outputs to prevent dead-code elimination.
- Compare baselines before accepting optimization complexity.

## Concurrency

- Goroutine owner and shutdown condition explicit.
- **`wg.Go(fn)`** for goroutine-per-task (Go 1.25+) — replaces `wg.Add(1)` + `go func() { defer wg.Done(); ... }()`.
- Bounded worker pools/backpressure for bursty workloads.
- `errgroup`/`WaitGroup` to avoid orphan goroutines.
- Guard shared state with mutexes/atomics; document invariants.
- No copying structs with mutexes. No fire-and-forget goroutines in library code.

## Context and Cancellation

- Thread context through concurrent work and I/O.
- Respect cancellation in loops and blocking operations.
- Child goroutines stop when parent context is done.
- Explicit timeout intent in code and tests.
- Cause-aware cancellation APIs when supported and useful.

## Resource Cleanup

- `defer` close near acquisition.
- `t.Cleanup` for test resource lifecycle.
- Document caller cleanup responsibilities.
- No `os.Exit` in code under test. Stop tickers/timers explicitly.
