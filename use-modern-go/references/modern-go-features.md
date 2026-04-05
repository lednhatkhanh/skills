# Modern Go Features by Version

Use to choose version-appropriate syntax and stdlib APIs. Default Go 1.26+; constrain only when module target requires.

## Rules

- Detect module Go version before coding.
- Use features only up to the target version.
- Prefer stdlib helpers over manual patterns.

## Feature Matrix

### Go 1.13+
- `errors.Is`, `errors.As` for wrapped error matching.

### Go 1.18+
- `any` instead of `interface{}`.
- Generics where they reduce duplication without obscuring intent.
- `strings.Cut`, `bytes.Cut` for delimiter splitting.

### Go 1.19+
- Typed atomics: `atomic.Bool`, `atomic.Int64`, `atomic.Pointer[T]`.

### Go 1.20+
- `errors.Join` for combining independent errors.
- `context.WithCancelCause`, `context.Cause` for cause propagation.
- `strings.CutPrefix`, `strings.CutSuffix`, `strings.Clone`, `bytes.Clone`.

### Go 1.21+
- `min`, `max`, `clear` built-ins.
- `slices` and `maps` packages.
- `sync.OnceFunc`, `sync.OnceValue`.
- `log/slog` for structured logging.

### Go 1.22+
- `for i := range n` integer range loops.
- Fixed loop variable capture in closures.
- Enhanced `net/http` `ServeMux` patterns, `r.PathValue`.

### Go 1.23+
- Iterator-capable collection helpers.

### Go 1.24+
- Generic type aliases.
- `testing.B.Loop()` for cleaner benchmark loops. `testing.T.Context()` / `testing.B.Context()` for test-scoped cancellation.
- `os.Root` for directory-constrained filesystem access.
- `strings.SplitSeq`, `strings.FieldsSeq`, `bytes.SplitSeq`, `bytes.FieldsSeq` for iterator-based splitting.
- `encoding/json` `omitzero` struct tag — omits zero values (complements `omitempty`).
- `runtime.AddCleanup` — preferred over `runtime.SetFinalizer`.
- Experimental `testing/synctest` for deterministic concurrent testing.

### Go 1.25+
- **`sync.WaitGroup.Go(fn)`** — replaces `wg.Add(1)` + `go func() { defer wg.Done(); fn() }()`. Prefer for all new goroutine-per-task patterns.
- `testing/synctest` graduated from experimental — use for deterministic time/scheduling tests.
- Container-aware `GOMAXPROCS` defaults on Linux (reads cgroup CPU limits).
- `runtime/trace.FlightRecorder` for lightweight always-on trace capture.
- `T.Attr`, `B.Attr`, `F.Attr` for structured test attributes.
- `T.Output`, `B.Output`, `F.Output` as `io.Writer` for test output.
- `net/http.CrossOriginProtection` for Fetch-metadata CSRF protection.
- `go.mod` `ignore` directive for excluding directories from `./...` matching.
- `encoding/json/v2` experimental behind `GOEXPERIMENT=jsonv2` — do not adopt as default.

### Go 1.26+
- **`new(expression)`** — `new()` accepts expressions, not just types. `new(30)` returns `*int`, `new(true)` returns `*bool`, `new(T{})` returns `*T`. Prefer for pointer-to-value construction.
- **`errors.AsType[T](err)`** — generic type-safe replacement for `errors.As`. Returns `(T, bool)` without needing a target variable.
- Self-referential generic type constraints now allowed.
- Analyzer-backed `go fix` modernizers — prefer over hand-applied mechanical rewrites.
- Green Tea GC enabled by default (10-40% GC overhead reduction). No tuning for pre-1.26 GC behavior.
- `crypto/hpke` for standards-based Hybrid Public Key Encryption (RFC 9180).
- `log/slog.NewMultiHandler` for fan-out to multiple log handlers.
- `testing.ArtifactDir` for test output files.
- `io.ReadAll` ~2x faster with ~50% less memory.
- `simd/archsimd` experimental, opt-in only.
- `runtime/secret` experimental for secure erasure of secret data.

## Newer Versions

- Check `https://go.dev/doc/devel/release` for each target.
- Add APIs only when confirmed in the module target version.

## Fallback

- If a helper is unavailable, use the clearest compatible idiom.
- Prefer portability and readability over aggressive feature usage.
