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
Write or update a JavaScript/TypeScript skill that enforces modern, strict, safe, performant, and consistent engineering practices.

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
7. Prefer `type` over `interface` when either works; use `interface` only when its specific capabilities are required.
8. Default to optional chaining (`?.`) and nullish coalescing (`??`) where appropriate.
9. Enforce immutable operations by default and allow mutation only when explicitly required and documented.
10. Include lazy evaluation and deferred work patterns where they improve performance and do not harm clarity.
11. Address common correctness and performance pitfalls with explicit anti-pattern -> replacement guidance.

Non-negotiable rules to encode:
- Do not use `any` unless unavoidable and explicitly justified.
- Do not rely on implicit truthy/falsy checks for nullable data where precision matters.
- Enforce immutable operations by default; do not mutate inputs or shared state unless explicitly required and documented.
- Do not use `Array.prototype.forEach` for async control flow.
- Do not use `Array.prototype.sort` on shared arrays when mutation is unsafe.
- Do not introduce classes unless composition-based functions are insufficient.

Validation checklist to include in the skill:
- Type checking and strict config validation
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
