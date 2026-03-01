---
name: use-modern-javascript-typescript
description: Design, write, review, and refactor modern JavaScript and TypeScript with TypeScript-first guidance for safety, correctness, performance, and consistency. Use when implementing or reviewing JS/TS services, libraries, CLIs, frontend code, async workflows, type models, and refactors that require strict mode, erasable TypeScript syntax, function-first design, and modern language features.
---

# Use Modern JavaScript + TypeScript

Apply this skill to produce predictable, type-safe, and maintainable JS/TS code with function-first design and strict defaults.

## Quick Start

1. Confirm runtime, module system, and TypeScript version before coding.
2. Enable strictness (`strict` + `alwaysStrict`) and enforce erasable TS syntax.
3. Prefer function declarations for named logic and composition over classes.
4. Prefer `type` over `interface` unless interface-specific behavior is required.
5. Use modern operators (`?.`, `??`) and immutable patterns by default.
6. Load only the reference files needed for the task.

## Reference Routing

- Load [references/style-safety-and-correctness.md](references/style-safety-and-correctness.md) for coding rules, strictness, erasable TS constraints, and type-system conventions.
- Load [references/performance-and-lazy-evaluation.md](references/performance-and-lazy-evaluation.md) for lazy evaluation, concurrency choices, and optimization strategy.
- Load [references/pitfalls-and-anti-patterns.md](references/pitfalls-and-anti-patterns.md) for common failure modes and exact replacements.
- Load [references/source-basis.md](references/source-basis.md) for canonical source provenance.

## Workflow

1. Scope the change.
- Identify runtime assumptions (`node`, browser, bundler, ESM/CJS) and compatibility constraints.
- Identify API surface and behavior contracts before editing internals.

2. Set language guardrails.
- Enforce strict mode semantics.
- Enforce erasable TS syntax only.
- Enforce function-first design and type consistency.

3. Implement for correctness first.
- Write explicit nullability handling.
- Keep data immutable unless mutation is required and localized.
- Prefer exhaustive control flow for tagged unions and status states.

4. Optimize after correctness.
- Defer expensive work until needed.
- Choose lazy or concurrent execution only when semantics remain correct.
- Measure before and after when touching hot paths.

5. Validate quality gates.

```bash
# project-specific command names vary; run equivalents for:
pnpm -s tsc --noEmit
pnpm -s lint
pnpm -s test
```

Run additional checks when relevant:

```bash
pnpm -s test -- --runInBand          # deterministic async debugging
pnpm -s test -- --coverage           # branch/edge-case confidence
pnpm -s vitest bench                 # benchmark hot paths (or equivalent)
```

## Non-Negotiable Rules

- Use function declarations for named functions; avoid function expressions for named logic.
- Prefer functions and composition over classes.
- Prefer `type` over `interface` when either works.
- Keep TypeScript erasable-only; avoid TS-only runtime emit constructs.
- Use `unknown` instead of `any` unless unavoidable and justified.
- Use `?.` and `??` over fragile chained guards and `||` defaults.
- Avoid mutating shared arrays/objects; prefer non-mutating alternatives.
- Do not use `forEach` for async control flow.
- Do not silently swallow errors or rejected promises.

## Output Expectations

When applying this skill in a coding task:

1. State the runtime + module assumptions.
2. Call out strictness and erasable-syntax decisions.
3. Highlight correctness decisions around nullability, async flow, and mutation.
4. Mention performance decisions only when evidence or profiling supports them.
5. List what was validated (`typecheck`, `lint`, `test`, and any perf checks).
