---
name: use-nuqs
description: Implement, review, and refactor type-safe URL query state with nuqs in React 19+ and TypeScript 5.9+ codebases. If Next.js is involved, assume Next.js 16+ App Router only, and target current evergreen browsers without legacy compatibility workarounds.
---

# Use nuqs

Keep URL-backed state predictable, typed, shareable, and aligned between React clients and Next.js App Router server code. Never add legacy browser or older framework compatibility workarounds.

## Baseline

- React 19+, TypeScript 5.9+, evergreen browsers Chrome 146+/Firefox 148+/Safari 26+.
- Next.js 16+ App Router only when Next.js is present.
- No Pages Router, no Remix/React Router/TanStack Router, no legacy browser shims.

## Reference Routing

Load only what the task needs:

- [references/parser-and-setup.md](references/parser-and-setup.md) — adapter setup, parser selection, defaults, shared descriptors, custom parsers.
- [references/state-and-server-integration.md](references/state-and-server-integration.md) — `useQueryState`, `useQueryStates`, loaders, caches, Suspense, App Router server integration.
- [references/performance-navigation-and-limits.md](references/performance-navigation-and-limits.md) — history behavior, rate limiting, URL hygiene, browser limits.
- [references/debugging-testing-and-troubleshooting.md](references/debugging-testing-and-troubleshooting.md) — debug logs, testing adapters, parser tests, failure modes.
- [references/source-basis.md](references/source-basis.md) — canonical source provenance.

## Workflow

1. **Scope** — Identify what belongs in the URL (survives refresh, deep links, shareable). Secrets, bulk data, ephemeral state stay out.
2. **Parsers** — Centralize key names, parsers, defaults in a shared module. Typed parsers over string coercion. Validate business rules after parsing.
3. **Wire client** — `useQueryState` for single key, `useQueryStates` for atomic multi-key. Clear with `null`. Functional updates from previous state.
4. **Server integration** — Loaders/caches from `nuqs/server`. Await async `searchParams`. `shallow: false` only for intentional server re-renders. Pair with Suspense and `startTransition` when needed.
5. **Optimize** — `history: 'replace'` for ephemeral state, `'push'` for navigable state. Debounce search inputs, throttle rapid updates. `urlKeys` for shorter URLs. Hooks in smallest subtree.
6. **Validate**:

```bash
pnpm -s tsc --noEmit
pnpm -s lint
pnpm -s test
```

When relevant: `--runInBand` (isolate URL tests), `test searchParams` (focused), `test:e2e` (navigation).

## Non-Negotiable Rules

### Never

- `useQueryState`/`useQueryStates` outside client components
- Different parsers for the same query key across components
- Raw string state for non-string data when a typed parser exists
- `shallow: false` as default — only for intentional server re-renders
- `null` as controlled input `value` — use `withDefault('')` or `value ?? ''`
- Sensitive, bulky, or non-shareable state in the URL
- Assume parsers validate business rules or object shape
- Skip `eq` on custom non-primitive parsers
- Legacy browser or pre-React 19/pre-Next 16 compatibility workarounds

### Always

- Wrap app with `NuqsAdapter` for Next.js App Router
- Import server-safe parsers, loaders, caches from `nuqs/server`
- Shared parser descriptors or custom hooks over repeated inline setup
- `useQueryStates` for related params that must update atomically
- Schema validation after parsing when domain constraints matter
- `startTransition` does NOT imply `shallow: false` — configure both when server work needed

## Output

1. State parser and key contract being introduced or reused.
2. Call out client-only vs App Router client/server coordinated.
3. Mention history, shallow/server-render, rate-limiting decisions.
4. Include tests or verification notes for refresh, deep-link, back/forward.
5. State React/TypeScript/Next.js assumptions.
