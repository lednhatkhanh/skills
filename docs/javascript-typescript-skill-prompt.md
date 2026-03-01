# JavaScript + TypeScript Skill Prompt

Use this prompt when you want to generate or update the `use-modern-javascript-typescript` skill.

## Source Basis

Core sources:
- https://github.com/wshobson/agents/blob/main/plugins/javascript-typescript/skills/modern-javascript-patterns/SKILL.md
- https://github.com/wshobson/agents/blob/main/plugins/javascript-typescript/skills/typescript-advanced-types/SKILL.md
- https://www.typescriptlang.org/tsconfig/strict.html
- https://www.typescriptlang.org/tsconfig/erasableSyntaxOnly.html
- https://nodejs.org/docs/latest/api/typescript.html

Additional recommended sources:
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toSorted
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/EPSILON
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for-await...of
- https://www.typescriptlang.org/tsconfig/
- https://www.typescriptlang.org/docs/handbook/
- https://github.com/microsoft/TypeScript/wiki/Performance

## Rewritten Prompt

```text
Use the following as canonical sources for the skill:

Core:
- https://github.com/wshobson/agents/blob/main/plugins/javascript-typescript/skills/modern-javascript-patterns/SKILL.md
- https://github.com/wshobson/agents/blob/main/plugins/javascript-typescript/skills/typescript-advanced-types/SKILL.md
- https://www.typescriptlang.org/tsconfig/strict.html
- https://www.typescriptlang.org/tsconfig/erasableSyntaxOnly.html
- https://nodejs.org/docs/latest/api/typescript.html

Additional:
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toSorted
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/EPSILON
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for-await...of
- https://www.typescriptlang.org/tsconfig/
- https://www.typescriptlang.org/docs/handbook/
- https://github.com/microsoft/TypeScript/wiki/Performance

Task:
Write or update a JavaScript/TypeScript skill that enforces modern, strict, safe, performant, and consistent engineering practices, including advanced type-system guidance.

Requirements:
1. Target both JavaScript and TypeScript, with TypeScript-first guidance.
2. Keep guidance practical, enforceable, and reviewable.
3. Prefer function declarations over function expressions for named logic.
4. Prefer functions and composition over classes by default.
5. Enforce strictness:
   - JavaScript strict mode semantics
   - TypeScript `strict: true` and `alwaysStrict: true`
6. Enforce erasable TypeScript syntax only:
   - disallow runtime TS constructs that require emit transforms (for example `enum`, runtime `namespace`, parameter properties, `import =`, `export =`, angle-bracket assertions)
7. Use deliberate type modeling rules:
   - prefer `type` for unions, transformations, and composition
   - use `interface` for extendable object contracts and declaration-merging cases
8. Include advanced TypeScript patterns with practical constraints:
   - constrained/defaulted generics
   - conditional types, mapped types, template literal types, and `infer`
   - explicit guidance on when advanced type-level abstraction is justified
9. Require runtime-safe narrowing at boundaries:
   - prefer type guards and assertion functions over broad `as` casting
10. Default to optional chaining (`?.`) and nullish coalescing (`??`) where appropriate.
11. Enforce immutable operations by default and allow mutation only when explicitly required and documented.
12. Include lazy evaluation and deferred work patterns where they improve performance and do not harm clarity.
13. Address common correctness and performance pitfalls with explicit anti-pattern -> replacement guidance.
14. Include type-level performance guardrails to prevent compile-time slowdowns from overly complex recursive types.

Non-negotiable rules to encode:
- Do not use `any` unless unavoidable and explicitly justified.
- Do not ship advanced utility types without compile-time usage tests for expected and invalid cases.
- Do not rely on implicit truthy/falsy checks for nullable data where precision matters.
- Enforce immutable operations by default; do not mutate inputs or shared state unless explicitly required and documented.
- Do not use `Array.prototype.forEach` for async control flow.
- Do not use `Array.prototype.sort` on shared arrays when mutation is unsafe.
- Do not introduce classes unless composition-based functions are insufficient.
- Do not use unchecked type assertions where runtime validation or narrowing is required.
- Do not leave discriminated unions non-exhaustive.

Validation checklist to include in the skill:
- Type checking and strict config validation
- Compile-time tests for non-trivial utility types (`@ts-expect-error`, `tsd`, or equivalent)
- Linting
- Unit tests for behavior and edge cases
- Async error-path tests
- Performance checks when touching hot paths

Output format:
1. Updated SKILL.md text.
2. Updated references sections (or file-level diffs) for style/safety, performance/lazy evaluation, pitfalls, and source-basis.
3. Short rationale of major changes.
4. Explicit assumptions (runtime, module system, TypeScript target, compatibility constraints).
```
