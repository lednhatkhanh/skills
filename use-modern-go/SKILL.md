---
name: use-modern-go
description: Design, write, review, and refactor maintainable, scalable, testable, readable, and modern Go code. Use when working on Go services, libraries, CLIs, APIs, concurrency, testing, performance, package architecture, module/dependency strategy, reliability, and production readiness; default to Go 1.26+ practices while keeping explicit version-aware guidance available when an older module target is relevant.
---

# Use Modern Go

Produce idiomatic Go code that is easy to evolve, safe under load, and operationally predictable. Never add compatibility shims for older Go versions unless the module explicitly targets one.

## Baseline

- Go 1.26+ unless the repository explicitly declares an older module target.
- Detect version: `go list -m -f '{{.GoVersion}}' 2>/dev/null || awk '/^go /{print $2; exit}' go.mod`
- Priorities in order: clarity, simplicity, concision, maintainability, consistency.

## Reference Routing

Load only what the task needs:

- [references/go-style-and-design.md](references/go-style-and-design.md) — naming, package/API design, errors, context, observability, security, data semantics, dependencies.
- [references/go-testing-and-concurrency.md](references/go-testing-and-concurrency.md) — tests, fuzzing, benchmarks, race safety, goroutine lifecycle, cancellation, cleanup.
- [references/modern-go-features.md](references/modern-go-features.md) — version-gated features from Go 1.13 through 1.26+.
- [references/source-basis.md](references/source-basis.md) — canonical source provenance.

## Workflow

1. **Scope** — Identify ownership boundaries, exported API impact, compatibility risks. Confirm Go version baseline.
2. **Design** — Cohesive small packages. Interfaces at consumers. Zero values useful. `context.Context` as first param for request-scoped work.
3. **Implement** — Return errors with `%w` and context. Explicit goroutine lifetime, bounded and cancelable. Structured logging at boundaries. No hidden global mutable state.
4. **Optimize** — Measure first. Benchmarks/profiles for hot paths. Justify `sync/atomic` or `sync.Pool` with data.
5. **Validate**:

```bash
go mod tidy                          # when deps/imports changed
gofmt -w ./...
go test ./...
go vet ./...
```

When relevant:

```bash
go test -race ./...                  # concurrency changes
go test -fuzz=Fuzz -run=^$ ./...     # parser/decoder/input-heavy code
go test -bench . ./...               # performance-sensitive paths
govulncheck ./...                    # reachable vuln checks (if installed)
```

## Non-Negotiable Rules

### Never

- Features newer than the target Go version
- Pre-1.26 compatibility branches unless module explicitly targets older
- `util`, `common`, `helpers`, or catch-all packages
- Clever abstractions hiding core control flow
- Swallowed errors — return rich actionable context with `%w`
- `context.Context` stored in structs
- Goroutines without ownership, cancellation, and shutdown paths
- Breaking exported API contracts unless explicitly requested and documented
- Dependencies without clear benefit and maintenance cost awareness

### Always

- Simple explicit control flow
- Interfaces defined at consumption boundaries, not preemptively
- Error handling with `errors.Is`/`errors.As` (`errors.AsType[T]` on 1.26+), wrapped with `%w`
- `sync.WaitGroup.Go(fn)` for goroutine-per-task patterns (Go 1.25+)
- Context propagated through call chains and goroutines
- Structured logs at I/O boundaries with stable identifiers
- Validate external input at boundaries; fail closed
- Document cleanup responsibilities (`Close`, `Stop`, `Cancel`)
- Protect API compatibility; favor additive evolution

## Output

1. Explain key design decisions and tradeoffs.
2. Call out version-gated decisions only when module target differs from Go 1.26+ default.
3. Include tests for behavior, edge cases, and failure paths.
4. State what was validated (`test`, `vet`, `race`, `fuzz`, `bench`, `govulncheck`).
5. State assumptions (Go version baseline, integration boundaries).
