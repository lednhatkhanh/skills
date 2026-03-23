# nuqs Skill Prompt

Use this prompt when you want to generate or update a React + TypeScript `nuqs` skill.

## Source Basis

Core sources:
- https://skills.sh/pproenca/dot-skills/nuqs
- https://nuqs.dev/docs/basic-usage
- https://nuqs.dev/docs/tips-tricks
- https://nuqs.dev/docs/debugging
- https://nuqs.dev/docs/testing
- https://nuqs.dev/docs/troubleshooting
- https://nuqs.dev/docs/server-side
- https://nuqs.dev/docs/limits

## Rewritten Prompt

```text
Use the following as canonical sources for the skill:

Core:
- https://skills.sh/pproenca/dot-skills/nuqs
- https://nuqs.dev/docs/basic-usage
- https://nuqs.dev/docs/tips-tricks
- https://nuqs.dev/docs/debugging
- https://nuqs.dev/docs/testing
- https://nuqs.dev/docs/troubleshooting
- https://nuqs.dev/docs/server-side
- https://nuqs.dev/docs/limits

Task:
Write or update a `nuqs` skill that enforces type-safe URL query state management practices for React applications written in TypeScript, with Next.js guidance limited to the App Router.

Scope:
- React only
- TypeScript only
- If Next.js is present, support Next.js 16+ App Router only
- Exclude Pages Router, Remix, React Router, TanStack Router, and JavaScript-only guidance unless needed to mark them as out of scope

Requirements:
1. Keep guidance practical, enforceable, and reviewable.
2. Preserve the full rule coverage implied by the community nuqs skill's 8 categories, but filter every rule through the React + TypeScript + Next.js App Router scope.
3. Cover parser configuration thoroughly:
   - use typed parsers for non-string data
   - use `withDefault` for non-nullable state
   - use enum or literal parsers for constrained values
   - cover arrays, JSON, ISO dates/timestamps, index offsets, and hex values
   - explain when structured data must be validated outside the parser with a schema validator
4. Cover setup and adapter rules:
   - `useQueryState` and `useQueryStates` only in client components with `'use client'`
   - when Next.js App Router is used, wrap the root with `NuqsAdapter` from `nuqs/adapters/next/app`
   - import shared server-safe parsers, loaders, and caches from `nuqs/server`
   - keep shared parser descriptors in dedicated modules for reuse between client and server
   - assume Next.js 16+ and do not add compatibility branches for older Next.js releases
5. Cover state-management rules:
   - clear params with `null`
   - prefer shared hook abstractions per key/parser pair
   - do not use different parsers for the same query key in different places
   - use functional updates when derived from previous value
   - use `useQueryStates` for related params and atomic updates
   - use the setter return value when code needs the flushed `URLSearchParams`
   - use parser-level `withOptions` when behavior should be shared across uses
6. Cover server integration for App Router:
   - `createLoader` and `createSearchParamsCache`
   - call `parse` or `loadSearchParams` before `get` or `all`
   - treat App Router `searchParams` as async and model examples accordingly
   - use `strict: true` when invalid query values must fail fast
   - use `shallow: false` only when server re-rendering or refetching is intended
   - pair server-triggering URL updates with `startTransition` and Suspense when needed
   - reuse the same parser declarations across server and client
7. Cover performance and URL hygiene:
   - debounce search-like inputs
   - throttle rapid URL updates
   - respect browser History API limits and Safari throttling behavior
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
   - testing with `withNuqsTestingAdapter` or `NuqsTestingAdapter`
   - parser bijection testing helpers
   - Suspense boundary guidance around client components that use nuqs
10. Cover advanced patterns selectively:
   - custom parsers with `createParser`
   - `eq` for non-primitive values
   - custom hooks that encapsulate shared parser setup
   - `urlKeys` as an explicit mapping layer, not a replacement for domain names in code

Non-negotiable rules to encode:
- Do not use `useQueryState` or `useQueryStates` outside React client components.
- Do not include Next.js Pages Router implementation guidance in the skill.
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
4. Explicit assumptions (React version, TypeScript usage, whether Next.js App Router is present).
5. Any out-of-scope items intentionally excluded (Next.js <=15, Pages Router, non-React frameworks, JavaScript-only examples).
```
