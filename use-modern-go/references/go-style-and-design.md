# Go Style and Design

Patterns and conventions that extend the rules in SKILL.md. Rules from SKILL.md are not repeated here.

## Naming

- Lower-case package names, no underscores. Short, descriptive, specific.
- Short receiver names (1-2 letters), consistent per type.
- No `Get`/`Set` prefixes unless domain language requires them.
- Don't repeat package name in exported symbols.
- Consistent initialism casing: `ID`, `URL`, `HTTP`.
- Variable name length proportional to scope.
- Don't shadow standard package names.

## Package and Module Design

- Organize around domain behavior, not technical layers.
- Minimal, stable public surface. Hide internals with unexported symbols.
- `internal/` for ownership boundaries when helpful.
- Explicit acyclic dependency direction.
- One module per deployable boundary unless strong reason to split.

## API Design

- Make zero values useful. Make invalid states hard to represent.
- Favor additive evolution over breaking changes.
- Use option structs when argument lists grow.
- Return concrete types from constructors unless abstraction is required.
- Keep side effects explicit in names and docs.

## Types and Methods

- Value receivers for small immutable-like types. Pointer receivers for mutation, large structs, or method-set consistency.
- Consistent receiver style per type.
- Prefer concrete types over `map[string]any` for stable schemas.
- Use generics when they reduce duplication and remain readable.
- Always specify channel direction in parameters when possible.

## Interfaces

- Define where consumed, not where produced.
- Small, behavior-oriented.
- No speculative abstraction before multiple implementations exist.
- Prefer concrete parameters until substitution is needed.

## Error Handling

- `error` as last return value. Handle immediately; early returns.
- Wrap with `%w` and actionable context. Place `%w` at end of format string.
- `errors.Is`/`errors.As` for semantic checks (`errors.AsType[T]` on Go 1.26+). Never match error strings.
- Use sentinel errors and typed errors for structured error contracts.
- Lowercase error strings, no punctuation.
- No duplicate log-and-return at the same boundary.

## Context

- First parameter for request-scoped operations.
- Propagate through call chains and goroutines.
- Respect cancellation/deadlines in blocking paths.
- `context.Background()` only at true process entrypoints.

## Observability

- Structured logs at boundaries (I/O, RPC, retries, failures).
- Sparse in hot paths; prefer metrics/tracing for high-volume signals.
- Stable identifiers (`request_id`, `user_id`, `job_id`).
- No secrets, tokens, or raw sensitive payloads in logs.
- Timeouts, retries, and backoff explicit and configurable.

## Security

- Validate and normalize external input at boundaries. Fail closed.
- Allowlists over blocklists. Authn/authz close to request boundaries.
- Least-privilege defaults for credentials and dependency scope.

## Data Semantics

- Explicit nil vs empty semantics.
- Nil slices/maps for absence unless API contract requires empty.
- Clone/copy when ownership crosses package or goroutine boundaries.
- Document mutability and ownership expectations.

## Comments

- Doc comments for exported symbols, starting with symbol name.
- Explain invariants, constraints, side effects.
- Document cleanup responsibilities and error contracts.

## Dependencies

- Stdlib first; third-party intentionally with clear justification.
- Minimize transitive surface. Pin and review updates.
- `go mod tidy` when dependency or import surfaces change.

## Performance

- Optimize after measurement. Benchmarks for hot paths.
- CPU/memory/block/mutex profiles before major refactors.
- No micro-optimizations that reduce readability without measurable gain.
