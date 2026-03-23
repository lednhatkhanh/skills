---
name: use-modern-go
description: Design, write, review, and refactor maintainable, scalable, testable, readable, and modern Go code. Use when working on Go services, libraries, CLIs, APIs, concurrency, testing, performance, package architecture, module/dependency strategy, reliability, and production readiness; default to Go 1.26+ practices while keeping explicit version-aware guidance available when an older module target is relevant.
---

# Use Modern Go

Apply this skill to produce idiomatic Go code that is easy to evolve, safe under load, and operationally predictable.

## Quick Start

1. Assume Go 1.26+ unless the repository explicitly declares an older module target.

```bash
go list -m -f '{{.GoVersion}}' 2>/dev/null || awk '/^go /{print $2; exit}' go.mod
```

2. Use Go 1.26+ language and stdlib features by default; only constrain lower when the module target explicitly requires it.
3. Protect API compatibility unless a breaking change is explicitly requested.
4. Prioritize, in order: clarity, simplicity, concision, maintainability, consistency.
5. Load only the reference files needed for the task.

## Reference Routing

- Load [references/go-style-and-design.md](references/go-style-and-design.md) for package/API design, naming, docs, errors, context, dependency policy, observability, and compatibility.
- Load [references/go-testing-and-concurrency.md](references/go-testing-and-concurrency.md) for tests, fuzzing, race safety, goroutine lifecycle, cancellation, and reliability checks.
- Load [references/modern-go-features.md](references/modern-go-features.md) when the task is version-specific or when an older module target is explicitly relevant.
- Load [references/source-basis.md](references/source-basis.md) for canonical source provenance.

## Workflow

1. Scope the change.
- Identify ownership boundaries, exported API impact, and compatibility risks first.
- Confirm whether the module is on the default Go 1.26+ baseline or explicitly pinned older before design choices.

2. Choose design direction.
- Keep packages cohesive and small.
- Define interfaces where consumed, not where produced.
- Keep zero values useful when practical.
- Accept `context.Context` as first parameter for request-scoped work.

3. Implement with predictable behavior.
- Return errors for expected failures; avoid panic in normal paths.
- Wrap errors with `%w` and actionable context.
- Keep goroutine lifetime explicit, bounded, and cancelable.
- Avoid hidden global mutable state.
- Keep logging structured and boundary-oriented; avoid duplicate log-and-return chains.

4. Optimize only with evidence.
- Measure before optimization.
- Use benchmarks/profiles for hot paths.
- Justify any `sync/atomic` or `sync.Pool` usage with measurable impact.

5. Validate with quality gates.

```bash
go mod tidy                          # when dependencies/imports changed
gofmt -w ./...
go test ./...
go vet ./...
```

Run additional checks when relevant:

```bash
go test -race ./...                  # concurrency changes
go test -run TestName ./...          # focused debugging
go test -fuzz=Fuzz -run=^$ ./...     # parser/decoder/input-heavy code
go test -bench . ./...               # performance-sensitive paths
govulncheck ./...                    # reachable vulnerability checks (if installed)
```

## Non-Negotiable Rules

- Do not use features newer than the target Go version.
- Do not add pre-1.26 compatibility branches or downgrade workarounds unless the module explicitly targets an older Go version.
- Do not introduce `util`, `common`, `helpers`, or catch-all packages.
- Do not hide core control flow in clever abstractions.
- Do not swallow errors; return rich actionable context.
- Do not store `context.Context` in structs.
- Do not start goroutines without ownership, cancellation, and shutdown paths.
- Do not break exported API contracts unless explicitly requested and documented.
- Do not add dependencies without clear benefit and maintenance cost awareness.

## Output Expectations

When applying this skill in a coding task:

1. Explain key design decisions and tradeoffs.
2. Call out version-gated decisions only when the module target is not the default Go 1.26+ baseline.
3. Include tests for behavior, edge cases, and failure paths.
4. Mention what was validated (`test`, `vet`, `race`, `fuzz`, `bench`, `govulncheck` as applicable).
5. State assumptions (default Go 1.26+ baseline or explicit older module target, plus integration boundaries).
