# State and Server Integration

Client query state wiring and Next.js App Router server patterns. Rules from SKILL.md are not repeated here.

## Single-Key State

```tsx
'use client'
import { parseAsInteger, useQueryState } from 'nuqs'

export function Pager() {
  const [page, setPage] = useQueryState('page', parseAsInteger.withDefault(1))
  return (
    <>
      <button onClick={() => setPage(v => v - 1)}>Previous</button>
      <button onClick={() => setPage(v => v + 1)}>Next</button>
      <button onClick={() => setPage(null)}>Reset</button>
    </>
  )
}
```

- Query string is source of truth — avoid duplicating into local `useState`.
- Nullable parsers: never pass `null` to input `value`; use `value ?? ''` or `withDefault('')`.
- Hook state updates immediately; URL flushing may be queued/throttled.

## Multi-Key State

```tsx
'use client'
import { parseAsInteger, parseAsString, useQueryStates } from 'nuqs'

const filterParsers = {
  query: parseAsString.withDefault(''),
  page: parseAsInteger.withDefault(1)
}

export function SearchControls() {
  const [{ query, page }, setFilters] = useQueryStates(filterParsers, {
    urlKeys: { query: 'q', page: 'p' }
  })
  return (
    <input value={query} onChange={e => setFilters({ query: e.target.value, page: 1 })} />
  )
}
```

- Atomic updates for related params. Partial objects for subsets, `null` to clear group.
- Option precedence: call-level > parser-level > hook-level defaults.

## Cross-Component Sync

- Same-key hooks stay synchronized across components automatically.
- Derive alternate views from one canonical parser — never redefine the key.
- Avoid mirroring query state into local `useState`.

## Setter Semantics

- Setters return a `Promise<URLSearchParams>` — use only when code needs the committed URL.
- Multiple setters in same tick may share one pending Promise and flush as single URL update.

## Server Loaders

```ts
// search-params.ts
import { createLoader, parseAsInteger, parseAsString } from 'nuqs/server'
export const searchParsers = { q: parseAsString.withDefault(''), page: parseAsInteger.withDefault(1) }
export const loadSearchParams = createLoader(searchParsers)
```

```tsx
// app/page.tsx
import type { SearchParams } from 'nuqs/server'
import { loadSearchParams } from './search-params'

type PageProps = { searchParams: Promise<SearchParams> }

export default async function Page({ searchParams }: PageProps) {
  const filters = await loadSearchParams(searchParams, { strict: true })
  return <div>{filters.q}</div>
}
```

- Reuse same parser descriptors on client and server.
- `strict: true` when invalid query values should fail.

## Search Params Cache

```ts
import { createSearchParamsCache, parseAsInteger, parseAsString } from 'nuqs/server'
export const searchParsers = { q: parseAsString.withDefault(''), page: parseAsInteger.withDefault(1) }
export const searchParamsCache = createSearchParamsCache(searchParsers)
```

- Call `parse` before `get`/`all`. Keep cache descriptor in shared module.

## Suspense and Transitions

- Wrap nuqs client components in `<Suspense>` when rendered from server page boundary.
- Pass `startTransition` as a direct hook option for pending/loading state:

```tsx
const [isPending, startTransition] = React.useTransition()
const [query, setQuery] = useQueryState('q', parseAsString.withOptions({
  startTransition,
  shallow: false
}))
```

- `startTransition` does NOT trigger server re-renders — `shallow: false` must be explicit.
