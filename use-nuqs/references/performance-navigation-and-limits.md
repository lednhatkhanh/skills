# Performance, Navigation, and Limits

URL update behavior, history semantics, and browser constraints. Rules from SKILL.md are not repeated here.

## History

| Scenario | Setting |
|---|---|
| Ephemeral UI (filters, typing, sorting, sliders) | `history: 'replace'` |
| Navigable state (tabs, steps, map locations) | `history: 'push'` |

Change `scroll` only for intentional viewport movement.

## Server Re-Render Control

- Client-first by default. `shallow: false` only for intentional server component re-rendering/refetching.
- Treat `shallow: false` as a behavior change, not a harmless default.

## Rate Limiting

- Use `limitUrlUpdates` with `throttle(ms)` or `debounce(ms)` from `nuqs` (replaces deprecated `throttleMs`).
- Default throttle: `50ms` most browsers, `120ms` Safari. Minimum 50ms enforced.
- Hook state stays responsive; URL flushes and server notifications are rate-limited.

```ts
import { debounce, parseAsString } from 'nuqs'
parseAsString.withOptions({ shallow: false, limitUrlUpdates: debounce(300) })
```

Use `throttle(...)` when every intermediate change matters at a bounded rate.

## URL Hygiene

- `clearOnDefault` deliberately. `urlKeys` only to shorten public URL surface.
- `createSerializer` over ad hoc string concatenation for links outside hooks.
- Hooks in smallest practical subtree; memoize expensive siblings.
- Reconsider design if URLs need complex objects, large JSON, or opaque payloads.

## URL Length

- URL is a constrained transport surface, not unlimited state store.
- Problems start in low-thousands-of-characters range (sharing, email, social tools).
- Safari more restrictive than Firefox/Chrome.
- Rework state model if query strings grow too large.
