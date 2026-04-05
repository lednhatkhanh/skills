# JavaScript + TypeScript Skill Prompt

Use this prompt when you want to generate or update the `use-modern-javascript-typescript` skill.

## Source Basis

Core sources:
- https://github.com/wshobson/agents/blob/main/plugins/javascript-typescript/skills/modern-javascript-patterns/SKILL.md
- https://github.com/wshobson/agents/blob/main/plugins/javascript-typescript/skills/typescript-advanced-types/SKILL.md
- https://www.typescriptlang.org/tsconfig/strict.html
- https://www.typescriptlang.org/tsconfig/erasableSyntaxOnly.html
- https://nodejs.org/api/typescript.html

Additional recommended sources:
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toSorted
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toReversed
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toSpliced
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/with
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/groupBy
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/groupBy
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Iterator
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/using
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/withResolvers
- https://developer.mozilla.org/en-US/docs/Web/API/Window/structuredClone
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort
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
- https://nodejs.org/api/typescript.html

Additional:
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toSorted
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toReversed
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toSpliced
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/with
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/groupBy
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/groupBy
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Iterator
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/using
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/withResolvers
- https://developer.mozilla.org/en-US/docs/Web/API/Window/structuredClone
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/EPSILON
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for-await...of
- https://www.typescriptlang.org/tsconfig/
- https://www.typescriptlang.org/docs/handbook/
- https://github.com/microsoft/TypeScript/wiki/Performance

Task:
Write or update a JavaScript/TypeScript skill that enforces modern, strict, safe, performant, and consistent engineering practices, including advanced type-system guidance. The skill targets AI coding agents, not humans — keep it concise, directive, and enforceable.

Requirements:
1. Target both JavaScript and TypeScript, with TypeScript-first guidance.
2. Default the skill to TypeScript 5.9+, Node.js 24+, and evergreen browsers Chrome 146+, Firefox 148+, and Safari 26+.
3. When React or Next.js are relevant, assume React 19+ and Next.js 16+ App Router only.
4. Keep guidance practical, enforceable, and reviewable. Optimize for agent consumption — directive rules, not explanatory prose.
5. Never write fallback code, polyfills, or compatibility branches for old browsers or older runtimes. This is a non-negotiable rule. Always use modern syntax and APIs.
6. Prefer ESM and modern platform capabilities; do not add legacy browser/runtime compatibility branches, polyfill advice, or CommonJS-first guidance unless explicitly requested.
7. Prefer function declarations over function expressions for named logic.
8. Prefer functions and composition over classes by default.
9. Enforce strictness:
   - JavaScript strict mode semantics
   - TypeScript `strict: true` and `alwaysStrict: true`
10. Enforce erasable TypeScript syntax only:
    - disallow runtime TS constructs that require emit transforms (for example `enum`, runtime `namespace`, parameter properties, `import =`, `export =`, angle-bracket assertions)
11. Use deliberate type modeling rules:
    - prefer `type` for unions, transformations, and composition
    - use `interface` for extendable object contracts and declaration-merging cases
12. Include advanced TypeScript patterns with practical constraints:
    - constrained/defaulted generics
    - conditional types, mapped types, template literal types, and `infer`
    - `satisfies` operator for validation without widening
    - `const` type parameters for literal inference
    - `NoInfer<T>` to control inference positions
    - explicit guidance on when advanced type-level abstraction is justified
13. Require runtime-safe narrowing at boundaries:
    - prefer type guards and assertion functions over broad `as` casting
    - do not use one-sided nullish guards; for concrete runtime type checks use `typeof value === "..."`, for explicit nullish checks handle both branches (`typeof value === "undefined" || value === null` or the inverse), and use `!!value` only when truthiness is the actual condition
14. Default to optional chaining (`?.`) and nullish coalescing (`??`) where appropriate.
15. Prefer strict equality:
    - use `===` and `!==` instead of `==` and `!=`
16. Enforce immutable operations by default and allow mutation only when explicitly required and documented.
17. Require modern immutable array methods (`toSorted`, `toReversed`, `toSpliced`, `.with()`) instead of mutating equivalents.
18. Require modern APIs where they replace legacy patterns:
    - `Object.groupBy()` / `Map.groupBy()` instead of manual reduce grouping
    - `Set` composition methods (`union`, `intersection`, `difference`, `symmetricDifference`, `isSubsetOf`, `isSupersetOf`, `isDisjointFrom`)
    - Iterator helpers (`.map()`, `.filter()`, `.take()`, `.drop()`, `.flatMap()`, `.reduce()`, `.toArray()`) for lazy pipelines
    - `using` / `await using` for resource cleanup (Explicit Resource Management)
    - `structuredClone()` for deep copies
    - `Promise.withResolvers()` for deferred promises
    - `URL.parse()` for non-throwing URL parsing
    - Import attributes (`import ... with { type: "json" }`)
19. Include lazy evaluation and deferred work patterns where they improve performance and do not harm clarity.
20. Address common correctness and performance pitfalls with explicit anti-pattern -> replacement guidance.
21. Include type-level performance guardrails to prevent compile-time slowdowns from overly complex recursive types.
22. Include TS compiler performance guidance: prefer interfaces over intersections, explicit return types on exports, skipLibCheck, avoid barrel files, isolatedDeclarations.

Non-negotiable rules to encode:
- Never write fallback code, polyfills, or compatibility branches for old browsers or pre-Node 24 runtimes.
- Always use modern syntax and APIs. Never use legacy equivalents when modern ones exist.
- Do not use `any` unless unavoidable and explicitly justified.
- Do not ship advanced utility types without compile-time usage tests for expected and invalid cases.
- Do not use one-sided nullish checks; use `typeof`-based narrowing for runtime type checks, handle both `undefined` and `null` when checking nullish presence/absence, and use `!!value` only when truthiness is the intended condition.
- Do not use `==` or `!=`; prefer `===` and `!==`.
- Enforce immutable operations by default; do not mutate inputs or shared state unless explicitly required and documented.
- Do not use `Array.prototype.forEach` for async control flow.
- Do not use `Array.prototype.sort` on shared arrays when mutation is unsafe; use `toSorted`.
- Do not introduce classes unless composition-based functions are insufficient.
- Do not use unchecked type assertions where runtime validation or narrowing is required.
- Do not leave discriminated unions non-exhaustive.
- Do not use CommonJS. ESM only.

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
4. Explicit assumptions (Node.js 24+, TypeScript 5.9+, supported browsers, ESM baseline, and React/Next.js assumptions when relevant).
```
