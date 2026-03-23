# Parser and Setup

Use this reference when introducing nuqs, defining query contracts, or reviewing parser correctness.

## Table of Contents

- 1) Scope Baseline
- 2) Adapter and Component Boundaries
- 3) Shared Parser Modules
- 4) Parser Selection Rules
- 5) Defaults and Nullability
- 6) Custom Parsers
- 7) Setup Review Checklist

## 1) Scope Baseline

- Stay in React only.
- Stay in TypeScript only.
- If Next.js is present, assume Next.js 16+ and use App Router only.
- Treat Next.js Pages Router guidance as out of scope for this skill.

## 2) Adapter and Component Boundaries

- Use `useQueryState` and `useQueryStates` only in client components with `'use client'`.
- Keep `page.tsx` and `layout.tsx` server-first when possible; move nuqs hooks into client children.
- In Next.js App Router, wrap the application tree with `NuqsAdapter` from `nuqs/adapters/next/app`.
- Import parser utilities needed by server code from `nuqs/server` to avoid pulling client-only boundaries into server modules.

Prefer:

```tsx
// app/layout.tsx
import { NuqsAdapter } from 'nuqs/adapters/next/app'
import type { ReactNode } from 'react'

export default function RootLayout({ children }: { children: ReactNode }) {
  return (
    <html lang="en">
      <body>
        <NuqsAdapter>{children}</NuqsAdapter>
      </body>
    </html>
  )
}
```

## 3) Shared Parser Modules

- Define parsers once in a dedicated module such as `search-params.ts`.
- Reuse the same descriptor object in client hooks, loaders, and caches.
- Abstract repeated key/parser pairs into a custom hook when multiple components need the same contract.
- Keep semantic variable names in code even if the URL uses shorter aliases.

Prefer:

```ts
import { parseAsInteger, parseAsString } from 'nuqs/server'

export const searchParsers = {
  q: parseAsString.withDefault(''),
  page: parseAsInteger.withDefault(1)
}
```

## 4) Parser Selection Rules

- Use raw string state only for true free-form string parameters.
- Use typed parsers for non-string values:
  - `parseAsInteger` for whole numbers
  - `parseAsFloat` for decimals
  - `parseAsBoolean` for booleans
  - `parseAsTimestamp` or `parseAsIsoDateTime` for `Date` values
  - enum/literal parsers for constrained states
  - `parseAsArrayOf(...)` for arrays
  - `parseAsJson<T>()` only when the state genuinely belongs in the URL and the shape is validated separately
- Use `parseAsIndex` when the UI presents 1-based numbering but internal logic should stay 0-based.
- Use `parseAsHex` for hexadecimal color-like values instead of hand-rolled parsing.
- Keep one serialization format per key. Do not let different callers invent different array or object encodings.

Prefer:

```ts
import { parseAsInteger, parseAsStringLiteral } from 'nuqs'

const statusValues = ['open', 'closed'] as const

const filterParsers = {
  page: parseAsInteger.withDefault(1),
  status: parseAsStringLiteral(statusValues).withDefault('open')
}
```

Avoid:

```ts
const [page] = useQueryState('page')
const pageNumber = Number(page ?? '1')
```

## 5) Defaults and Nullability

- Use `withDefault` when consumers should receive a non-nullable state value.
- Remember that default values stay internal unless the feature intentionally writes them to the URL.
- Clear a value with `null`; this removes the key from the URL and falls back to the default value if one exists.
- Use `clearOnDefault: false` only when the feature explicitly needs default values visible in the URL.

## 6) Custom Parsers

- Use `createParser` for domain-specific types.
- Keep `parse`, `serialize`, and `eq` pure.
- Return `null` for invalid inputs.
- Add an `eq` function whenever primitive equality is insufficient, such as for `Date` or object values.
- Validate business invariants after parsing with a schema validator or explicit checks.

Prefer:

```ts
import { createParser } from 'nuqs'

type DateRange = {
  start: string
  end: string
}

export const parseAsDateRange = createParser<DateRange>({
  parse(value) {
    const [start, end] = value.split('..')
    if (!start || !end) {
      return null
    }
    return { start, end }
  },
  serialize(value) {
    return `${value.start}..${value.end}`
  },
  eq(a, b) {
    return a.start === b.start && a.end === b.end
  }
})
```

## 7) Setup Review Checklist

- Is the codebase React + TypeScript?
- If Next.js is present, is the implementation Next.js 16+ App Router only?
- Is `NuqsAdapter` mounted at the appropriate root?
- Are server-side imports coming from `nuqs/server`?
- Is every non-string query key using a typed parser?
- Is each key/parser pair centralized and reused?
- Are defaults intentional and null-clearing semantics understood?
