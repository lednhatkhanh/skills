# Pitfalls and Anti-Patterns

Use this reference during implementation and review to prevent common JS/TS bugs and regressions.

## Async and Promise Pitfalls

1. Pitfall: `array.forEach(async () => ...)` and expecting completion ordering.
- Risk: `forEach` does not await async callbacks.
- Fix: use `for...of` with `await`, or `await Promise.all(array.map(...))`.

2. Pitfall: `Promise.all` for operations where partial success is acceptable.
- Risk: one rejection fails the entire aggregate.
- Fix: use `Promise.allSettled` and reconcile results explicitly.

3. Pitfall: floating promises (unhandled rejections).
- Risk: hidden failures and process-level instability.
- Fix: always `await`, return, or explicitly handle with `.catch`.

## Data Mutation Pitfalls

1. Pitfall: calling `.sort()` on shared arrays.
- Risk: in-place mutation causes non-local side effects.
- Fix: use `toSorted()` or clone first (`[...arr].sort(...)`).

2. Pitfall: shallow clone assumptions (`{ ...obj }`, `[...arr]`).
- Risk: nested references still alias mutable data.
- Fix: deep clone intentionally when needed (`structuredClone`) or redesign ownership.

3. Pitfall: mutating inputs in utility functions.
- Risk: hard-to-track behavior regressions.
- Fix: return new values unless mutation is part of explicit contract.

## Type and Nullability Pitfalls

1. Pitfall: using `||` for defaults on nullable fields.
- Risk: valid falsy values (`0`, `false`, `""`) are overwritten.
- Fix: use `??`.

2. Pitfall: aggressive type assertions (`as SomeType`) instead of narrowing.
- Risk: compile-time silence with runtime failure.
- Fix: use type guards, schema validation, and `unknown` boundaries.

3. Pitfall: broad `any`.
- Risk: type safety collapse and hidden defects.
- Fix: prefer `unknown`, narrow early, and model domain types explicitly.

## Numeric and Date Pitfalls

1. Pitfall: direct equality checks on floating-point math.
- Risk: precision errors.
- Fix: compare with tolerance (`Math.abs(a - b) < Number.EPSILON * scale`).

2. Pitfall: mixing local time and UTC implicitly.
- Risk: timezone and DST bugs.
- Fix: normalize strategy per boundary (`Date`, `Intl`, or domain-specific temporal library).

## Control Flow and API Pitfalls

1. Pitfall: non-exhaustive handling of discriminated unions.
- Risk: missing states at runtime after type evolution.
- Fix: enforce exhaustive `switch` with `never` checks.

2. Pitfall: optional chaining used where missing data should be fatal.
- Risk: silent propagation of invalid state.
- Fix: use explicit invariants (`throw`, assertion function) at boundary points.

3. Pitfall: class-heavy designs for simple stateless logic.
- Risk: hidden mutable state, harder testing, and unnecessary indirection.
- Fix: use plain functions, module-level composition, and explicit state containers.

## Review Checklist

- Are all async flows awaited, returned, or captured?
- Are defaults using `??` rather than `||` when falsy values are valid?
- Are shared inputs protected from mutation?
- Are runtime boundaries validated before type narrowing?
- Are hot-path optimizations measured and documented?
