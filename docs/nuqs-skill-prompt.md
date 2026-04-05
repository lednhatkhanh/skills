# nuqs Skill Prompt

Use this prompt when you want to generate or update a React + TypeScript `nuqs` skill.

## Source Basis

Core sources:
- https://github.com/pproenca/dot-skills/blob/master/skills/.curated/nuqs/SKILL.md
- https://nuqs.dev/docs/basic-usage
- https://nuqs.dev/docs/options
- https://nuqs.dev/docs/batching
- https://nuqs.dev/docs/tips-tricks
- https://nuqs.dev/docs/debugging
- https://nuqs.dev/docs/testing
- https://nuqs.dev/docs/troubleshooting
- https://nuqs.dev/docs/server-side
- https://nuqs.dev/docs/limits
- https://nuqs.dev/docs/parsers/built-in
- https://nuqs.dev/docs/parsers/making-your-own

## Rewritten Prompt

```text
Use the following as canonical sources for the skill:

Core:
- https://github.com/pproenca/dot-skills/blob/master/skills/.curated/nuqs/SKILL.md
- https://nuqs.dev/docs/basic-usage
- https://nuqs.dev/docs/options
- https://nuqs.dev/docs/batching
- https://nuqs.dev/docs/tips-tricks
- https://nuqs.dev/docs/debugging
- https://nuqs.dev/docs/testing
- https://nuqs.dev/docs/troubleshooting
- https://nuqs.dev/docs/server-side
- https://nuqs.dev/docs/limits
- https://nuqs.dev/docs/parsers/built-in
- https://nuqs.dev/docs/parsers/making-your-own

Task:
Write or update a nuqs skill for type-safe URL query state in React + TypeScript with Next.js App Router. The skill targets AI coding agents — keep it concise, directive, and enforceable. Optimize for minimal context window usage.

Scope:
- React 19+, TypeScript 5.9+, Next.js 16+ App Router only, Chrome 146+/Firefox 148+/Safari 26+.
- Exclude Pages Router, Remix, React Router, TanStack Router, JavaScript-only, and legacy browser guidance.
- Never add fallback code or compatibility workarounds for older frameworks or browsers.

Requirements:
1. Practical, enforceable, agent-targeted. Directive rules, not explanatory prose.
2. Eliminate cross-file repetition. SKILL.md is single source for rules. References add depth (examples, patterns, config) without restating SKILL.md rules. No scope-baseline sections, TOC, or review checklists in references.
3. Cover parser configuration: typed parsers, `withDefault`, enum/literal, array formats, JSON with schema validation, dates, index offsets, hex. Parser selection as table.
4. Cover adapter: `NuqsAdapter` in root layout, `'use client'` boundaries, `nuqs/server` imports.
5. Cover state: `useQueryState` single-key, `useQueryStates` atomic multi-key, cross-component sync, setter semantics (Promise return), option precedence, controlled input null handling.
6. Cover server integration: `createLoader`, `createSearchParamsCache`, async `searchParams`, `strict: true`, `shallow: false` only for intentional server re-renders, Suspense + `startTransition` (does NOT imply `shallow: false`).
7. Cover performance: `history: 'replace'` vs `'push'` as table, debounce/throttle, default rates (50ms/120ms Safari), URL hygiene, length limits.
8. Cover debugging/testing: browser/server debug logs, `withNuqsTestingAdapter`, parser bijection, common failure modes as table.

Non-negotiable rules to encode:
- No hooks outside client components.
- No different parsers for same key.
- No raw strings for typed data.
- No `shallow: false` as default.
- No `null` as controlled input value.
- No sensitive/bulky state in URL.
- No skipping `eq` on non-primitive custom parsers.
- No legacy browser or pre-React-19/pre-Next-16 compatibility.
- Shared parser descriptors over inline setup.
- `useQueryStates` for atomic multi-key updates.
- Schema validation after parsing for business rules.

Output format:
1. Updated SKILL.md text.
2. Updated references for parser/setup, state/server, performance/navigation, debugging/testing, source-basis.
3. Short rationale of changes.
4. Explicit assumptions and out-of-scope items.
```
