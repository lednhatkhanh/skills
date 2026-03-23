# Source Basis

Last reviewed: 2026-03-23

## Canonical Inputs Used for This Skill

Primary upstream skills:

- https://github.com/wshobson/agents/blob/main/plugins/javascript-typescript/skills/modern-javascript-patterns/SKILL.md
- https://github.com/wshobson/agents/blob/main/plugins/javascript-typescript/skills/typescript-advanced-types/SKILL.md

Authoritative language and runtime references:

- https://www.typescriptlang.org/docs/handbook/
- https://www.typescriptlang.org/tsconfig/
- https://www.typescriptlang.org/tsconfig/strict.html
- https://www.typescriptlang.org/tsconfig/erasableSyntaxOnly.html
- https://github.com/microsoft/TypeScript/wiki/Performance
- https://nodejs.org/api/typescript.html
- https://developer.mozilla.org/en-US/docs/Web/JavaScript
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toSorted
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/EPSILON
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for-await...of

Referenced by upstream skill resource sections:

- https://javascript.info/
- https://github.com/getify/You-Dont-Know-JS
- https://eloquentjavascript.net/
- https://github.com/type-challenges/type-challenges
- https://basarat.gitbook.io/typescript/
- Effective TypeScript (Dan Vanderkam)

## Notes

- Upstream source skills did not include separate `references/` folders; their SKILL content and linked resources were used as reference basis.
- This skill applies TypeScript-first guidance while remaining usable for JavaScript-only codebases.
- Default baselines for this skill are TypeScript 5.9+, Node.js 24+, evergreen browsers Chrome 146+, Firefox 148+, and Safari 26+, plus React 19+ and Next.js 16+ App Router when relevant.
- Legacy browser/runtime compatibility guidance is intentionally excluded unless explicitly requested.
- Guidance from `typescript-advanced-types` is encoded as practical rules for constrained generics, conditional/mapped/template literal types, `infer`, runtime-safe narrowing, compile-time type tests, and type-check performance guardrails.
