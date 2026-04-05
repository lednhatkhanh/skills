---
name: use-modern-javascript-typescript
description: Design, write, review, and refactor modern JavaScript and TypeScript with TypeScript-first guidance for safety, correctness, performance, consistency, and advanced type modeling. Default to TypeScript 5.9+, Node.js 24+, React 19+, Next.js 16+ App Router when relevant, and current evergreen browsers only.
---

# Use Modern JavaScript + TypeScript

Produce predictable, type-safe, maintainable JS/TS with function-first design, strict defaults, and modern platform APIs. Never write fallback code for older runtimes or browsers.

## Baseline

- TypeScript 5.9+, Node.js 24+, ESM only.
- Evergreen browsers: Chrome 146+, Firefox 148+, Safari 26+.
- React 19+ and Next.js 16+ App Router when relevant.
- No polyfills, no CommonJS, no legacy transpilation, no compatibility branches.

## Reference Routing

Load only what the task needs:

- [references/style-safety-and-correctness.md](references/style-safety-and-correctness.md) — tsconfig, type modeling examples, modern API usage examples, erasable TS table.
- [references/performance-and-lazy-evaluation.md](references/performance-and-lazy-evaluation.md) — lazy evaluation, iterator helpers, concurrency, compiler perf.
- [references/pitfalls-and-anti-patterns.md](references/pitfalls-and-anti-patterns.md) — anti-pattern → fix lookup table.
- [references/source-basis.md](references/source-basis.md) — canonical source provenance.

## Workflow

1. **Scope** — Identify runtime target (Node/browser/framework). Default is always modern; never downgrade without explicit request.
2. **Guardrails** — Strict mode, erasable TS only, strict equality, function-first, ESM.
3. **Implement** — Correctness first. Modern APIs. Constrained generics, discriminated unions, `satisfies`, `const` type parameters.
4. **Optimize** — Defer expensive work. Iterator helpers for lazy pipelines. Measure hot paths. No compatibility shims.
5. **Validate** — Type-check, lint, test. Compile-time type tests for non-trivial utility types.

## Non-Negotiable Rules

### Never

- Fallback code, polyfills, or compatibility branches for old browsers/runtimes
- `any` (unless unavoidable + justified comment), `==`/`!=`, `||` for nullable defaults
- `forEach` for async control flow
- `.sort()`/`.reverse()`/`.splice()` on shared arrays — use `toSorted()`/`toReversed()`/`toSpliced()`
- `enum`, runtime `namespace`, parameter properties, `import =`/`export =`, angle-bracket assertions
- Unchecked `as` assertions where narrowing or validation is needed
- Non-exhaustive discriminated unions
- Silently swallowed errors or rejected promises
- Classes when functions and composition suffice

### Always

- Function declarations for named functions; arrows only for inline callbacks
- `type` for unions/transforms; `interface` for extendable object contracts
- `?.`, `??` over chained guards; `===`/`!==` only
- `typeof` for runtime type checks; both `undefined` and `null` for nullish checks; `!!value` only for truthiness
- Modern immutable arrays: `toSorted()`, `toReversed()`, `toSpliced()`, `.with()`
- Modern APIs: `Object.groupBy()`/`Map.groupBy()`, Set composition (`union`/`intersection`/`difference`/`symmetricDifference`/`isSubsetOf`/`isSupersetOf`/`isDisjointFrom`), iterator helpers (`.map()`/`.filter()`/`.take()`/`.drop()`/`.flatMap()`/`.reduce()`/`.toArray()`), `using`/`await using`, `structuredClone()`, `Promise.withResolvers()`, `URL.parse()`
- `satisfies` for type validation without widening; `const` type parameters and `NoInfer<T>` for inference control
- Constrained/defaulted generics; bounded and compile-time tested utility types
- All inputs immutable unless explicitly documented otherwise

## Validation

```bash
pnpm -s tsc --noEmit           # type-check
pnpm -s lint                   # lint
pnpm -s test                   # unit tests
```

When relevant: `--runInBand` (async debugging), `--coverage`, `vitest bench` (hot paths), `tsd` (type contract tests).

## Output

1. State runtime + module assumptions.
2. Call out strictness and erasable-syntax decisions.
3. Highlight correctness decisions (nullability, async, mutation, types).
4. Mention perf decisions only when evidence supports them.
5. List what was validated.
