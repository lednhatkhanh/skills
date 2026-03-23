# Go Skill Prompt

Use this prompt when you want to generate or update the `use-modern-go` skill.

## Source Basis

Core sources:
- https://go.dev/doc/effective_go
- https://google.github.io/styleguide/go/guide
- https://google.github.io/styleguide/go/decisions
- https://google.github.io/styleguide/go/best-practices
- https://go.dev/doc/faq
- https://github.com/JetBrains/go-modern-guidelines/blob/main/claude/modern-go-guidelines/skills/use-modern-go/SKILL.md

Additional recommended sources:
- https://go.dev/wiki/CodeReviewComments
- https://go.dev/ref/spec
- https://go.dev/ref/mem
- https://go.dev/ref/mod
- https://go.dev/doc/articles/race_detector
- https://go.dev/doc/security/fuzz/
- https://go.dev/doc/security/vuln/
- https://pkg.go.dev/testing
- https://pkg.go.dev/context
- https://go.dev/doc/devel/release

## Rewritten Prompt

```text
Use the following as canonical sources for the skill:

Core:
- https://go.dev/doc/effective_go
- https://google.github.io/styleguide/go/guide
- https://google.github.io/styleguide/go/decisions
- https://google.github.io/styleguide/go/best-practices
- https://go.dev/doc/faq
- https://github.com/JetBrains/go-modern-guidelines/blob/main/claude/modern-go-guidelines/skills/use-modern-go/SKILL.md

Additional:
- https://go.dev/wiki/CodeReviewComments
- https://go.dev/ref/spec
- https://go.dev/ref/mem
- https://go.dev/ref/mod
- https://go.dev/doc/articles/race_detector
- https://go.dev/doc/security/fuzz/
- https://go.dev/doc/security/vuln/
- https://pkg.go.dev/testing
- https://pkg.go.dev/context
- https://go.dev/doc/devel/release

Task:
Write or update a Go skill that enforces best practices for maintainable, scalable, testable, readable, and modern Go code.

Requirements:
1. Keep guidance practical, enforceable, and reviewable (avoid vague advice).
2. Default the skill to Go 1.26+ guidance unless a module explicitly targets an older Go version.
3. Cover package design, API boundaries, naming, error semantics, context usage, concurrency safety, testing strategy, and module/version management.
4. Keep `references/modern-go-features.md` as a version-complete history of best practices by Go version, including older releases.
5. In `SKILL.md` and non-version-history references, avoid legacy compatibility shims or downgrade guidance for pre-1.26 Go unless an older target version is explicitly relevant.
6. Include version-aware guidance for modern Go features and stdlib APIs; avoid suggesting features newer than the module's target Go version.
7. Prefer modern stdlib choices and current toolchain behavior for Go 1.26+ by default.
8. Include clear workflow steps, non-negotiable rules, quality gates, and output expectations.
9. Structure for progressive disclosure: concise SKILL.md + focused references.
10. Include security and reliability practices: race detection, fuzzing, vuln scanning, and safe defaults.
11. Include observability and operability guidance: structured logging, cancellation/timeout boundaries, and failure transparency.
12. Avoid speculative future-version claims; verify version-specific statements against official release notes.
13. Prefer minimal, targeted edits when updating an existing skill.

Non-negotiable rules to encode:
- Prefer simple, explicit control flow over clever abstractions.
- Define interfaces at consumption boundaries, not preemptively.
- Return rich wrapped errors; never silently swallow errors.
- Never store context.Context in structs; pass it explicitly.
- Do not spawn goroutines without ownership, cancellation, and shutdown strategy.
- Do not add pre-1.26 compatibility branches or downgrade workarounds unless an older module target is explicit.
- Keep APIs backward-compatible unless a breaking change is explicitly intended and documented.
- Avoid global mutable state unless strictly justified.

Optimization guidance to encode:
- Optimize after measurement, not by guesswork.
- Use benchmarks for performance-sensitive paths and compare before/after.
- Use profiling (CPU/memory/alloc/block/mutex) for bottlenecks before refactors.
- Prefer allocation reduction and data-layout improvements only when evidence shows impact.
- Add sync/atomic/sync.Pool guidance with caution and required justification.

Validation checklist to include in the skill:
- go mod tidy                          # when dependencies or imports changed
- gofmt -w ./...
- go test ./...
- go vet ./...
- go test -race ./...               # when concurrency is touched
- go test -fuzz=Fuzz -run=^$ ./...  # when parsers/decoders/input handlers are relevant
- go test -bench . ./...            # when performance is a concern
- govulncheck ./...                 # dependency and reachable vuln checks

Output format:
1. Updated SKILL.md text.
2. Updated reference file sections (or file-level diffs) for style/design, testing/concurrency, modern features, and source-basis.
3. Short rationale of major changes (what changed and why).
4. Explicit assumptions (default Go 1.26+ baseline, any older module target if explicitly relevant, and project type if inferred).
5. Note any commands that are optional based on tool availability (for example, govulncheck).
```
