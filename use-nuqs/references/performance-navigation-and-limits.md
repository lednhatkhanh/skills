# Performance, Navigation, and Limits

Use this reference when tuning URL update behavior, history semantics, or browser-facing constraints.

## Table of Contents

- 1) History Semantics
- 2) Server Re-Render Control
- 3) Rate Limiting and Input Behavior
- 4) URL Hygiene
- 5) URL Length Limits
- 6) Performance Review Checklist

## 1) History Semantics

- Use `history: 'replace'` for ephemeral UI state such as live filters, typing, sorting, or slider movement.
- Use `history: 'push'` when the user should be able to replay state changes with the browser back button.
- Change `scroll` only when the navigation should intentionally move the viewport.

Prefer:

```ts
parseAsString.withOptions({
  history: 'replace'
})
```

Use `push` for navigation-like state such as tabs, step flows, or map locations that users expect to revisit.

## 2) Server Re-Render Control

- Leave updates client-first by default.
- Set `shallow: false` only when the feature depends on App Router server component re-rendering, refetching, or server-side derivation.
- Treat `shallow: false` as a behavior change, not a harmless default.

## 3) Rate Limiting and Input Behavior

- Debounce search-like inputs before flushing query changes that trigger server work.
- Throttle rapid updates such as sliders, drag interactions, or repeated button presses when the URL changes at high frequency.
- Rely on nuqs' built-in queueing, but configure stricter limits when browser behavior or server load requires it.
- Remember the documented defaults:
  - nuqs queues URL updates and throttles them by default.
  - The default throttle is `50ms` on most browsers and `120ms` on Safari.
  - Hook state stays responsive immediately, while URL flushes and server notifications are the parts being rate-limited.

Prefer:

```ts
import { debounce, parseAsString } from 'nuqs'

parseAsString.withOptions({
  shallow: false,
  limitUrlUpdates: debounce(300)
})
```

Use `throttle(...)` rather than `debounce(...)` when every intermediate change should still be represented at a bounded rate.

## 4) URL Hygiene

- Use `clearOnDefault` deliberately.
- Keep semantic variable names in code and use `urlKeys` only to shorten the public URL surface.
- Prefer `createSerializer` or related serializer utilities over ad hoc string concatenation when generating links outside React hooks.
- Keep nuqs hook usage in the smallest practical subtree and memoize expensive siblings that do not depend on query state.
- Reconsider the design if the feature needs complex nested objects, large JSON blobs, or opaque transport payloads in the URL.

## 5) URL Length Limits

- Treat the URL as a constrained transport surface, not an unlimited state store.
- Expect practical problems once URLs move into the low-thousands-of-characters range, especially when links are copied into chat, email, or social tools.
- Assume browser and platform limits vary:
  - Chrome: very large theoretical limit, but practical issues can appear far earlier
  - Firefox: much larger than old legacy limits
  - Safari: more restrictive than Firefox/Chrome
- Rework the state model if query strings are growing large enough to hurt sharing, readability, or reliability.
- Ignore unsupported legacy-browser quirks outside the Chrome 146+ / Firefox 148+ / Safari 26+ support baseline.

## 6) Performance Review Checklist

- Does the feature need `push`, or is `replace` the better default?
- Does any update really need `shallow: false`?
- Are search-like or high-frequency inputs debounced/throttled appropriately?
- Are rate-limit defaults overridden only when the feature needs it?
- Are URLs concise, readable, and stable?
- Are `urlKeys` used only where they materially improve the URL?
- Are unrelated expensive components isolated from query-state re-renders?
- Is the amount of query-state data still appropriate for a shared URL?
