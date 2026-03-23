# Debugging, Testing, and Troubleshooting

Use this reference when diagnosing nuqs behavior, writing tests, or reviewing common integration failures.

## Table of Contents

- 1) Debug Logging
- 2) Component and Hook Testing
- 3) Custom Parser Testing
- 4) Common Failure Modes
- 5) Troubleshooting Checklist

## 1) Debug Logging

Enable browser logs during development:

```js
localStorage.setItem('debug', 'nuqs')
```

- Reload after setting the flag.
- Expect logs prefixed with `[nuqs]` and `[nuq+]`, plus browser User Timing markers for URL updates and renders.

Enable server or RSC logs in Node environments:

```bash
DEBUG=nuqs pnpm dev
```

- Use this when debugging App Router server rendering, `nuqs/server`, or cache/loader issues.
- Preserve relevant debug output when reporting or documenting a hard failure.
- Do not document or chase unsupported legacy-browser behavior unless the task explicitly asks for it.

## 2) Component and Hook Testing

- Use `withNuqsTestingAdapter` for React Testing Library hooks and components.
- Use `NuqsTestingAdapter` directly when a wrapper component reads more clearly than a higher-order helper.
- Seed tests with initial search params that match the scenario under test.
- Assert both rendered UI behavior and URL update behavior.
- Use `hasMemory: true` only when the test truly needs accumulated URL state across successive updates.
- Keep rate limiting disabled in tests unless timing behavior itself is under test; opt in deliberately with adapter options.

Prefer:

```tsx
import { render, screen } from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import {
  withNuqsTestingAdapter,
  type OnUrlUpdateFunction
} from 'nuqs/adapters/testing'
import { vi } from 'vitest'

it('updates the URL when the page changes', async () => {
  const user = userEvent.setup()
  const onUrlUpdate = vi.fn<OnUrlUpdateFunction>()

  render(<Pager />, {
    wrapper: withNuqsTestingAdapter({
      searchParams: '?page=2',
      onUrlUpdate
    })
  })

  await user.click(screen.getByRole('button', { name: /next/i }))

  expect(onUrlUpdate).toHaveBeenCalled()
})
```

## 3) Custom Parser Testing

- Treat custom parsers as pure units.
- Test `parse`, `serialize`, and `eq`.
- Verify bijection whenever the parser is meant to round-trip.
- Use helpers from `nuqs/testing` for explicit parser contract tests.

Prefer:

```ts
import { parseAsInteger } from 'nuqs'
import { isParserBijective } from 'nuqs/testing'

expect(isParserBijective(parseAsInteger, '42', 42)).toBe(true)
```

## 4) Common Failure Modes

1. Missing adapter setup.
- Symptom: hooks fail or URL state never syncs correctly.
- Fix: ensure `NuqsAdapter` is mounted for Next.js App Router.

2. Missing `'use client'`.
- Symptom: build or runtime failures from using hooks in a server component.
- Fix: move the hook into a client component and keep the page/layout server-first.

3. Missing Suspense boundary around client code rendered from a server page.
- Symptom: `Missing Suspense boundary with useSearchParams`.
- Fix: wrap the client child in `<Suspense>` instead of turning the whole page into a client component.

4. Different parsers on the same key.
- Symptom: confusing synchronized state or values that no longer match expectations.
- Fix: define one canonical parser per key and derive alternate views locally.

5. Reading from cache before parsing.
- Symptom: undefined or stale server values.
- Fix: call `parse` on the cache during the page render before calling `get` or `all`.

6. Assuming parsers validate business invariants.
- Symptom: structurally invalid or domain-invalid values leak into app logic.
- Fix: add schema validation or explicit checks after parsing.

7. Test runner/module setup hides nuqs imports.
- Symptom: Jest or older test configuration fails to load the package cleanly.
- Fix: remember that `nuqs` is ESM-only and configure the test runner accordingly before blaming the hooks.

## 5) Troubleshooting Checklist

- Is debug logging enabled in the right environment?
- Is the component definitely a client component?
- Is `NuqsAdapter` mounted?
- Is Suspense applied at the server/client boundary?
- Is one key mapped to exactly one parser contract?
- Are tests asserting both UI state and URL state where relevant?
- Are test-adapter options like `hasMemory`, `rateLimitFactor`, or queue-reset behavior set intentionally?
- Are custom parsers covered with round-trip or bijection tests?
