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
Write or update a `nuqs` skill that enforces type-safe URL query state management practices for React applications written in TypeScript, with Next.js guidance limited to the App Router.

Scope:
- React 19+ only
- TypeScript 5.9+ only
- If Next.js is present, support Next.js 16+ App Router only
- Assume evergreen browsers Chrome 146+, Firefox 148+, and Safari 26+
- Exclude Pages Router, Remix, React Router, TanStack Router, and JavaScript-only guidance unless needed to mark them as out of scope

Requirements:
1. Keep guidance practical, enforceable, and reviewable.
2. Preserve the full rule coverage implied by the community nuqs skill's 8 categories, but filter every rule through the React + TypeScript + Next.js App Router scope.
   - read the GitHub skill's referenced `references/*.md` files as the rule inventory, not just the top-level summary page
3. Cover parser configuration thoroughly:
   - use typed parsers for non-string data
   - use `withDefault` for non-nullable state
   - use enum or literal parsers for constrained values
   - cover array format choice between `parseAsArrayOf(...)` and `parseAsNativeArrayOf(...)`
   - cover JSON, ISO dates/timestamps, index offsets, and hex values
   - mention `parseAsIsoDate`, `parseAsIsoDateTime`, and `parseAsTimestamp`
   - explain that `parseAsJson(...)` should receive a Standard Schema or synchronous validator when shape validation matters
   - explain when structured data must still be validated outside the parser for business invariants
4. Cover setup and adapter rules:
   - `useQueryState` and `useQueryStates` only in client components with `'use client'`
   - when Next.js App Router is used, wrap the root with `NuqsAdapter` from `nuqs/adapters/next/app`
   - import shared server-safe parsers, loaders, and caches from `nuqs/server`
   - keep shared parser descriptors in dedicated modules for reuse between client and server
   - assume React 19+, TypeScript 5.9+, and Next.js 16+ with no compatibility branches for older releases
5. Cover state-management rules:
   - clear params with `null`
   - prefer shared hook abstractions per key/parser pair
   - do not use different parsers for the same query key in different places
   - controlled inputs must never receive `null`; use `withDefault('')` or `value={state ?? ''}` when binding inputs directly
   - controlled inputs may bind directly to nuqs state because local hook state updates immediately
   - use functional updates when derived from previous value
   - use `useQueryStates` for related params and atomic updates
   - use the setter return value when code needs the flushed `URLSearchParams`
   - use parser-level `withOptions` when behavior should be shared across uses
   - explain option precedence: call-level overrides parser-level, which overrides hook/global defaults
6. Cover server integration for App Router:
   - `createLoader` and `createSearchParamsCache`
   - call `parse` or `loadSearchParams` before `get` or `all`
   - treat App Router `searchParams` as async and model examples accordingly
   - use `strict: true` when invalid query values must fail fast
   - use `shallow: false` only when server re-rendering or refetching is intended
   - pair server-triggering URL updates with `startTransition` and Suspense when needed
   - make clear that `startTransition` does not replace `shallow: false`
   - reuse the same parser declarations across server and client
7. Cover performance and URL hygiene:
   - debounce search-like inputs
   - throttle rapid URL updates
   - note the default throttling behavior (`50ms` generally, `120ms` on Safari)
   - respect browser History API limits and Safari throttling behavior within the supported browser baseline
   - explain that hook state stays immediate while URL flushes and server notifications are rate-limited
   - isolate URL-dependent hook usage to the smallest practical subtree and memoize expensive siblings that do not depend on query state
   - use `clearOnDefault` deliberately
   - use `urlKeys` and serializer utilities when links or shorter URLs are needed
   - warn when query-state payloads are too large or unsuitable for the URL
8. Cover history and navigation behavior:
   - use `history: 'replace'` for ephemeral local state
   - use `history: 'push'` for navigation-like state where back/forward should replay changes
   - use `scroll` only when desired
   - explain synchronization across components bound to the same keys
9. Cover debugging, testing, and troubleshooting:
   - browser debug logs via `localStorage`
   - Node/RSC debug logs via `DEBUG=nuqs`
   - browser logs are prefixed with `[nuqs]` / `[nuq+]` and emit user-timing markers
   - testing with `withNuqsTestingAdapter` or `NuqsTestingAdapter`
   - parser bijection testing helpers
   - mention testing-adapter options like `hasMemory` and rate-limit controls when relevant
   - Suspense boundary guidance around client components that use nuqs
10. Cover advanced patterns selectively:
   - custom parsers with `createParser`
   - `eq` for non-primitive values
   - custom hooks that encapsulate shared parser setup
   - `urlKeys` as an explicit mapping layer, not a replacement for domain names in code
   - note that framework-specific adapters outside Next.js App Router are intentionally out of scope for this skill even though they exist upstream

Non-negotiable rules to encode:
- Do not use `useQueryState` or `useQueryStates` outside React client components.
- Do not include Next.js Pages Router implementation guidance in the skill.
- Do not add fallback guidance for unsupported legacy browsers or pre-React-19 / pre-Next-16 setups unless explicitly requested.
- Do not use raw string state for non-string query data when a typed parser exists.
- Do not configure different parsers for the same query key across components.
- Do not assume loaders, caches, or parsers validate business invariants; add schema validation where shape or domain constraints matter.
- Do not use `shallow: false` by default; use it only when server re-rendering is required.
- Do not store bulky, sensitive, or non-shareable application state in the URL.
- Do not skip `eq` for custom parsers whose values are not safely compared by primitive equality.
- Prefer shared parser objects or custom hooks over repeated inline parser declarations.
- Prefer `useQueryStates` over multiple separate hooks when parameters must change atomically.

Validation checklist to include in the skill:
- TypeScript typecheck
- ESLint
- Unit or component tests for components using nuqs
- Tests using `nuqs/adapters/testing` when URL behavior matters
- Parser round-trip or bijection tests for custom parsers
- Manual verification of back/forward navigation, deep links, refresh persistence, and Suspense/loading behavior
- Manual verification that URL length and update frequency remain reasonable for the feature

Output format:
1. Updated SKILL.md text.
2. Updated reference file sections (or file-level diffs) grouped by parser/setup, state/server integration, performance/navigation, debugging/testing, and source-basis.
3. Short rationale of major changes.
4. Explicit assumptions (React 19+, TypeScript 5.9+, Next.js 16+ App Router when present, and supported browser baseline).
5. Any out-of-scope items intentionally excluded (Next.js <=15, Pages Router, non-React frameworks, JavaScript-only examples, legacy browser compatibility).
```
