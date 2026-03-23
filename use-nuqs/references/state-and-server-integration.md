# State and Server Integration

Use this reference when wiring client query state, coordinating related params, or integrating nuqs with Next.js App Router server code.

## Table of Contents

- 1) Single-Key State
- 2) Multi-Key State
- 3) Cross-Component Synchronization
- 4) Setter Semantics
- 5) App Router Server Loaders
- 6) Search Params Cache
- 7) Suspense and Transitions
- 8) Integration Review Checklist

## 1) Single-Key State

- Use `useQueryState` for one query key.
- Clear the key with `null`.
- Use functional updates when the next value depends on the previous one.
- Treat the query string as the source of truth; avoid duplicating derived local state unless there is a strong reason.

Prefer:

```tsx
'use client'

import { parseAsInteger, useQueryState } from 'nuqs'

export function Pager() {
  const [page, setPage] = useQueryState('page', parseAsInteger.withDefault(1))

  return (
    <>
      <button onClick={() => setPage(value => value - 1)}>Previous</button>
      <button onClick={() => setPage(value => value + 1)}>Next</button>
      <button onClick={() => setPage(null)}>Reset</button>
    </>
  )
}
```

## 2) Multi-Key State

- Use `useQueryStates` when keys must change together.
- Prefer one atomic update over several separate hook updates for related params.
- Use partial object updates for subsets and `null` to clear the whole managed group.
- Use `urlKeys` when the URL should stay short but the code should remain descriptive.

Prefer:

```tsx
'use client'

import { parseAsInteger, parseAsString, useQueryStates } from 'nuqs'

const filterParsers = {
  query: parseAsString.withDefault(''),
  page: parseAsInteger.withDefault(1)
}

export function SearchControls() {
  const [{ query, page }, setFilters] = useQueryStates(filterParsers, {
    urlKeys: {
      query: 'q',
      page: 'p'
    }
  })

  return (
    <input
      value={query}
      onChange={event => setFilters({ query: event.target.value, page: 1 })}
    />
  )
}
```

## 3) Cross-Component Synchronization

- Remember that hooks bound to the same query key stay synchronized across components.
- Do not configure different parsers for the same key in different places.
- If consumers need different derived views of the same state, derive them from one canonical parser instead of redefining the key.

## 4) Setter Semantics

- Nuqs state updates return a Promise that resolves to the flushed `URLSearchParams`.
- Use that Promise only when code truly needs the committed URL, such as analytics, sharing, or follow-up logic.
- Assume UI state updates immediately, while URL flushing may be queued or rate-limited.

## 5) App Router Server Loaders

- Use `createLoader` from `nuqs/server` to parse search params on the server.
- Reuse the same parser descriptor object that the client uses.
- In Next.js 16+ App Router, treat `searchParams` as async and await the loader result.
- Use strict mode when invalid query input should fail instead of silently falling back.

Prefer:

```ts
// search-params.ts
import { createLoader, parseAsInteger, parseAsString } from 'nuqs/server'

export const searchParsers = {
  q: parseAsString.withDefault(''),
  page: parseAsInteger.withDefault(1)
}

export const loadSearchParams = createLoader(searchParsers)
```

```tsx
// app/page.tsx
import type { SearchParams } from 'nuqs/server'
import { loadSearchParams } from './search-params'

type PageProps = {
  searchParams: Promise<SearchParams>
}

export default async function Page({ searchParams }: PageProps) {
  const filters = await loadSearchParams(searchParams, { strict: true })
  return <div>{filters.q}</div>
}
```

## 6) Search Params Cache

- Use `createSearchParamsCache` when nested server components need typed search params without prop drilling.
- Call `parse` before calling `get` or `all`.
- Keep the cache descriptor in the same shared module as the parser declarations.

Prefer:

```ts
import {
  createSearchParamsCache,
  parseAsInteger,
  parseAsString
} from 'nuqs/server'

export const searchParsers = {
  q: parseAsString.withDefault(''),
  page: parseAsInteger.withDefault(1)
}

export const searchParamsCache = createSearchParamsCache(searchParsers)
```

```tsx
import type { SearchParams } from 'nuqs/server'
import { searchParamsCache } from './search-params'

type PageProps = {
  searchParams: Promise<SearchParams>
}

export default async function Page({ searchParams }: PageProps) {
  await searchParamsCache.parse(searchParams)
  return <Results />
}

function Results() {
  const query = searchParamsCache.get('q')
  return <div>{query}</div>
}
```

## 7) Suspense and Transitions

- Wrap client components that use nuqs in `<Suspense>` when rendering them from a server page boundary.
- Keep the outer shell server-rendered when possible; move the client component down the tree.
- Use `startTransition` with server-triggering updates when the feature needs pending/loading state visibility.
- Use `shallow: false` only for intentional server re-renders or refetching.

## 8) Integration Review Checklist

- Is single-key vs multi-key state chosen intentionally?
- Are related params updated atomically when required?
- Are duplicate local derivations avoided?
- Are loader/cache imports coming from `nuqs/server`?
- Is `parse` called before `get`/`all` on caches?
- Is async `searchParams` handled correctly for Next.js 16+ App Router?
- Are Suspense and transitions used only where they add visible value?
