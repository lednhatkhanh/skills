---
name: use-nuqs
description: Implement, review, and refactor type-safe URL query state with nuqs in React + TypeScript codebases. Use when adding or debugging query-string-backed UI state, parser definitions, shared search-param descriptors, App Router server/client integration, URL update behavior, or tests for components using useQueryState and useQueryStates. If Next.js is involved, assume Next.js 16+ and apply only to App Router patterns.
---

# Use nuqs

Apply this skill to keep URL-backed state predictable, typed, shareable, and aligned between React clients and Next.js App Router server code.

## Quick Start

1. Confirm the task is in a React + TypeScript codebase.
2. If Next.js is involved, assume Next.js 16+, stay in App Router only, and keep `page.tsx`/`layout.tsx` server-first unless client behavior is required.
3. Reuse existing parser descriptors or custom hooks before adding new query keys.
4. Choose a typed parser and default value before wiring UI state.
5. Use `useQueryStates` when related params must update atomically.
6. Enable server re-renders only when the feature genuinely depends on server-side data or RSC updates.
7. Load only the reference files needed for the task.

## Scope Guards

- Support React only.
- Support TypeScript only.
- Support Next.js 16+ App Router only when Next.js is present.
- Exclude Next.js Pages Router, Remix, React Router, TanStack Router, and JavaScript-only variants unless explicitly documenting them as out of scope.

## Reference Routing

- Load [references/parser-and-setup.md](references/parser-and-setup.md) for adapter setup, parser selection, defaults, shared descriptors, and custom parser rules.
- Load [references/state-and-server-integration.md](references/state-and-server-integration.md) for `useQueryState`, `useQueryStates`, loaders, caches, Suspense, and App Router server integration.
- Load [references/performance-navigation-and-limits.md](references/performance-navigation-and-limits.md) for history behavior, rate limiting, URL hygiene, shorter keys, and browser limits.
- Load [references/debugging-testing-and-troubleshooting.md](references/debugging-testing-and-troubleshooting.md) for debug logs, testing adapters, parser tests, and common failure modes.
- Load [references/source-basis.md](references/source-basis.md) for canonical source provenance.

## Workflow

1. Scope the query-state contract.
- Identify which state belongs in the URL because it must survive refresh, deep links, shareable URLs, or browser navigation.
- Keep secrets, oversized payloads, and purely ephemeral state out of the URL.

2. Define the parser layer first.
- Centralize key names, parsers, defaults, and shared options in a dedicated module.
- Prefer built-in parsers over ad hoc string coercion.
- Add schema validation after parsing when domain constraints matter.

3. Wire client state deliberately.
- Use `useQueryState` for a single key.
- Use `useQueryStates` for coordinated params, partial updates, or atomic commits.
- Clear keys with `null`.
- Use functional updates when deriving from previous state.

4. Integrate App Router server behavior only when needed.
- Import loaders, caches, and shared parsers from `nuqs/server`.
- Parse `searchParams` before reading cached values in server components.
- Use `shallow: false` only when the URL update must trigger server work.
- Pair server-triggered updates with Suspense and `startTransition` when loading state matters.

5. Optimize URL update behavior.
- Use `history: 'replace'` for ephemeral filters and high-frequency edits.
- Use `history: 'push'` for navigation-like state that should replay with back/forward.
- Debounce search-like inputs and throttle rapid updates when browser limits or server churn matter.
- Keep URLs short and stable with defaults, `urlKeys`, and serializer utilities when needed.

6. Validate the behavior end to end.

```bash
# run project-specific equivalents as available
pnpm -s tsc --noEmit
pnpm -s lint
pnpm -s test
```

Run additional checks when relevant:

```bash
pnpm -s test -- --runInBand        # isolate flaky URL/state tests
pnpm -s test searchParams          # focused component or hook tests
pnpm -s test:e2e                   # back/forward, refresh, and deep-link verification
```

## Non-Negotiable Rules

- Use `useQueryState` and `useQueryStates` only in React client components.
- Wrap the app with `NuqsAdapter` when using Next.js App Router.
- Import server-safe parsers, loaders, and caches from `nuqs/server`.
- Do not use different parsers for the same query key across components.
- Do not keep non-string query data as raw strings when a typed parser exists.
- Do not assume nuqs parsers validate business rules or object shape.
- Do not default to `shallow: false`; use it only for intentional server re-renders.
- Do not put sensitive, bulky, or non-shareable state in the URL.
- Do not skip `eq` on custom non-primitive parsers.
- Prefer shared parser descriptors or custom hooks over repeating inline parser setup.

## Output Expectations

When applying this skill in a coding task:

1. State the parser and key contract being introduced or reused.
2. Call out whether the change is client-only or App Router client/server coordinated.
3. Mention history, shallow/server-render, and rate-limiting decisions when they affect behavior.
4. Include tests or manual verification notes for refresh, deep-link, and back/forward behavior.
5. State assumptions about React, TypeScript, and whether Next.js App Router is present.
