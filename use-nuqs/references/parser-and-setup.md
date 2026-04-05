# Parser and Setup

Adapter configuration, parser selection, and shared descriptor patterns. Rules from SKILL.md are not repeated here.

## Adapter

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

- Keep `page.tsx`/`layout.tsx` server-first; move nuqs hooks into client children.
- Import server-side parsers from `nuqs/server`.
- Global defaults via adapter (v2.5.0+):

```tsx
<NuqsAdapter defaultOptions={{ shallow: false, scroll: true, clearOnDefault: false, limitUrlUpdates: throttle(250) }}>
```

## Shared Parser Modules

Define parsers once, reuse across client hooks, loaders, and caches:

```ts
import { parseAsInteger, parseAsString } from 'nuqs/server'

export const searchParsers = {
  q: parseAsString.withDefault(''),
  page: parseAsInteger.withDefault(1)
}
```

Abstract repeated key/parser pairs into custom hooks when multiple components share the same contract.

## Parser Selection

| Data type | Parser |
|---|---|
| Free-form string | `parseAsString` (noop — switch to literal/enum if constrained) |
| Integer | `parseAsInteger` |
| Decimal | `parseAsFloat` |
| Boolean | `parseAsBoolean` |
| Hex integer | `parseAsHex` |
| 1-based index | `parseAsIndex` (0-based internally, +1 in URL) |
| Date | `parseAsIsoDate`, `parseAsIsoDateTime`, `parseAsTimestamp` |
| String literal union | `parseAsStringLiteral(values)` |
| Number literal union | `parseAsNumberLiteral(values)` |
| String enum | `parseAsStringEnum(enum)` (uses `Object.values`) |
| Separator array | `parseAsArrayOf(parser, separator?)` (default comma) |
| Repeated-key array | `parseAsNativeArrayOf(parser)` (`?tag=a&tag=b` format) |
| JSON blob | `parseAsJson(schema)` — Standard Schema (Zod/ArkType/Valibot) or custom validator |

One serialization format per key. Never let different callers invent different encodings.

## Defaults and Nullability

- `withDefault(value)` for non-nullable state.
- Clear with `null` — removes key, falls back to default.
- `clearOnDefault: false` only when default must be visible in URL.
- `withOptions(...)` close to shared parser declarations for inherited behavior.

## Custom Parsers

```ts
import { createParser } from 'nuqs'

export const parseAsDateRange = createParser<DateRange>({
  parse(value) {
    const [start, end] = value.split('..')
    if (!start || !end) return null
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

- `parse`, `serialize`, `eq` must be pure. Return `null` for invalid inputs.
- `eq` required for non-primitive values.
- Validate business invariants after parsing, not inside the parser.
- Bijective when round-tripping matters.
