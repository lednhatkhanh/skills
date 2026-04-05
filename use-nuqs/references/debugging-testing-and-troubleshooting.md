# Debugging, Testing, and Troubleshooting

Diagnosing nuqs behavior, writing tests, and common failure fixes. Rules from SKILL.md are not repeated here.

## Debug Logging

Browser: `localStorage.setItem('debug', 'nuqs')` then reload. Logs prefixed `[nuqs]`/`[nuq+]` + User Timing markers.

Server/RSC: `DEBUG=nuqs pnpm dev`

## Component and Hook Testing

```tsx
import { render, screen } from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import { withNuqsTestingAdapter, type OnUrlUpdateFunction } from 'nuqs/adapters/testing'
import { vi } from 'vitest'

it('updates URL on page change', async () => {
  const user = userEvent.setup()
  const onUrlUpdate = vi.fn<OnUrlUpdateFunction>()
  render(<Pager />, {
    wrapper: withNuqsTestingAdapter({ searchParams: '?page=2', onUrlUpdate })
  })
  await user.click(screen.getByRole('button', { name: /next/i }))
  expect(onUrlUpdate).toHaveBeenCalled()
})
```

- `NuqsTestingAdapter` directly when wrapper reads better.
- Seed initial search params. Assert both UI and URL behavior.
- `hasMemory: true` only when test needs accumulated URL state.
- Disable rate limiting unless testing timing behavior.

## Custom Parser Testing

```ts
import { parseAsInteger } from 'nuqs'
import { isParserBijective } from 'nuqs/testing'
expect(isParserBijective(parseAsInteger, '42', 42)).toBe(true)
```

Test `parse`, `serialize`, `eq`. Verify bijection for round-trip parsers.

## Common Failure Modes

| Symptom | Cause | Fix |
|---|---|---|
| Hooks fail / URL never syncs | Missing `NuqsAdapter` | Mount adapter in root layout |
| Build/runtime error from hooks in server component | Missing `'use client'` | Move hook to client component |
| `Missing Suspense boundary` error | No Suspense around client child | Wrap in `<Suspense>` |
| Confusing synchronized state | Different parsers on same key | One canonical parser per key |
| Undefined/stale server values | Cache read before parse | Call `parse` before `get`/`all` |
| Invalid values leak into logic | Assuming parser validates business rules | Add schema validation after parsing |
| Jest/test runner fails to load nuqs | ESM-only package | Configure test runner for ESM |
