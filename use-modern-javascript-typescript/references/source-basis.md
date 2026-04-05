# Source Basis

Last reviewed: 2026-04-05

## Canonical Inputs

Primary upstream skills:

- https://github.com/wshobson/agents/blob/main/plugins/javascript-typescript/skills/modern-javascript-patterns/SKILL.md
- https://github.com/wshobson/agents/blob/main/plugins/javascript-typescript/skills/typescript-advanced-types/SKILL.md

TypeScript references:

- https://www.typescriptlang.org/docs/handbook/
- https://www.typescriptlang.org/tsconfig/
- https://www.typescriptlang.org/tsconfig/strict.html
- https://www.typescriptlang.org/tsconfig/erasableSyntaxOnly.html
- https://github.com/microsoft/TypeScript/wiki/Performance
- https://nodejs.org/api/typescript.html

MDN JavaScript references:

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

Community references:

- https://javascript.info/
- https://github.com/getify/You-Dont-Know-JS
- https://eloquentjavascript.net/
- https://github.com/type-challenges/type-challenges
- https://basarat.gitbook.io/typescript/
- Effective TypeScript (Dan Vanderkam)

## Notes

- Default baselines: TypeScript 5.9+, Node.js 24+, evergreen browsers Chrome 146+, Firefox 148+, Safari 26+, React 19+ and Next.js 16+ App Router when relevant.
- ESM only. No CommonJS guidance.
- Legacy browser/runtime compatibility guidance intentionally excluded.
- Modern JS APIs (immutable array methods, Object.groupBy, Map.groupBy, Set composition, iterator helpers, using/await using, Promise.withResolvers, structuredClone) are baseline — not optional.
- Modern TS features (satisfies, const type parameters, NoInfer, erasableSyntaxOnly, isolatedDeclarations, verbatimModuleSyntax) are baseline.
- Upstream skills did not include `references/` folders; their content and linked resources were used as reference basis.
