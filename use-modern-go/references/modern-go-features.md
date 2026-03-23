# Modern Go Features by Version

Use this file to choose version-appropriate syntax and stdlib APIs.

## Rules

- Detect and honor the module Go version before coding.
- Default to Go 1.26+ unless the repository explicitly targets an older version.
- Use features only up to and including the target version.
- Prefer clearer standard library helpers over manual patterns.
- If uncertain about feature availability, check the release notes for that version.

## Go 1.13+

- Use `errors.Is` and `errors.As` for wrapped error matching.

## Go 1.18+

- Use `any` instead of `interface{}` where appropriate.
- Use generics where they reduce duplication without obscuring intent.
- Use `strings.Cut` and `bytes.Cut` for delimiter splitting.

## Go 1.19+

- Use typed atomics (`atomic.Bool`, `atomic.Int64`, `atomic.Pointer[T]`) when atomic coordination is required.

## Go 1.20+

- Use `errors.Join` for combining independent errors.
- Use `context.WithCancelCause` and `context.Cause` when cause propagation is relevant.
- Use `strings.CutPrefix` and `strings.CutSuffix` for explicit prefix/suffix parsing.
- Use `strings.Clone` and `bytes.Clone` when ownership isolation is needed.

## Go 1.21+

- Use `min`, `max`, and `clear` built-ins when they improve clarity.
- Use `slices` and `maps` helper packages for common collection operations.
- Use `sync.OnceFunc` and `sync.OnceValue` for one-time initialization helpers.
- Use `log/slog` for structured logging where it fits the project conventions.

## Go 1.22+

- Use `for i := range n` when integer range loops improve readability.
- Rely on fixed loop variable capture behavior in closures.
- Use enhanced `net/http` `ServeMux` patterns and `r.PathValue` when available.

## Go 1.23+

- Use iterator-capable collection helpers where they simplify code.

## Go 1.24+

- Use generic type aliases where they reduce duplication without obscuring the API surface.
- Use the new `testing.B.Loop()` helper for cleaner benchmark loops when it improves readability.
- Use `testing.T.Context()` / `testing.B.Context()` when test-scoped cancellation or deadlines should flow into helpers.
- Use `os.Root` when code needs safer filesystem access constrained to a directory tree.

## Go 1.25+

- Use `testing/synctest` for deterministic testing of concurrent code when time and scheduling behavior need tighter control.
- Assume container-aware `GOMAXPROCS` defaults on Linux instead of manually forcing CPU counts in typical containerized deployments.
- Use the `go.mod` `ignore` directive when generated, vendored, or unrelated directories should stay out of `./...` package matching.
- Treat `encoding/json/v2` and `encoding/json/jsontext` as experimental behind `GOEXPERIMENT=jsonv2`; do not adopt them as normal defaults unless the project explicitly opts in.

## Go 1.26+

- Use analyzer-backed `go fix` modernizers during upgrade and cleanup work before hand-applying broad mechanical rewrites.
- Use `crypto/hpke` for HPKE support instead of third-party implementations when the project needs standards-based Hybrid Public Key Encryption.
- Treat `simd/archsimd` as experimental and opt-in only; do not make it a default dependency path in general application code.
- Assume the new garbage collector and other toolchain/runtime improvements are the default baseline behavior; do not add tuning advice that exists only to preserve older pre-1.26 behavior unless measurement justifies it.

## For Newer Versions

- Review `https://go.dev/doc/devel/release` for each target version.
- Add newer APIs only when confirmed available in the module target version.

## Fallback Guidance

- If a helper is unavailable, use the clearest compatible idiom.
- Prefer portability and readability over aggressive feature usage.
- Mention version constraints only when the choice is non-obvious.
