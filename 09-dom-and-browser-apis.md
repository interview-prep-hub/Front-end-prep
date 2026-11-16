# 09. DOM and browser APIs

Previous: [08. Async](08-javascript-async-and-event-loop.md) | Next: [10. Coding patterns](10-data-structures-and-javascript-coding.md)

## Events and the DOM

Know event capture, target, and bubble phases. `event.target` is where an event originated; `currentTarget` is where the current listener is attached. `preventDefault` stops a cancelable default action; `stopPropagation` affects propagation. Neither is a general replacement for the other.

Event delegation lets one ancestor handle many dynamic descendants. Validate targets and keep the lookup inside the intended container.

```js
const list = document.querySelector('#tasks');
const controller = new AbortController();
list.addEventListener('click', (event) => {
  if (!(event.target instanceof Element)) return;
  const button = event.target.closest('button[data-task-id]');
  if (!button || !list.contains(button)) return;
  console.log('Complete task', button.dataset.taskId);
}, { signal: controller.signal });
// During teardown:
// controller.abort();
```

Use `textContent` to insert plain text. `innerHTML` interprets markup, so untrusted content needs an explicit sanitization strategy. Native form submission, selection, focus, and scroll behavior are part of your feature, even if a library owns rendering.

## Storage and communication

| API | Useful for | Important boundary |
| --- | --- | --- |
| localStorage | Small persistent preferences | Synchronous; script-readable; can fail; not a secret store |
| sessionStorage | Per-tab session UI state | Lifetime and scope differ from localStorage |
| IndexedDB | Larger structured offline records | Async schema/version handling and conflict rules |
| Cookies | Server-visible session identifiers | Scope, HttpOnly, Secure, SameSite, and CSRF design |
| BroadcastChannel | Coordinating eligible browsing contexts | Validate messages; never treat them as server authorization |
| History API | Same-document navigation state | Router owns it in most applications |

Storage quota, private modes, policy, and user actions can affect persistence. Wrap storage access where failure is possible. Scope persisted UI by account/organization when it could expose or confuse private data after a switch.

## Observers and workers

IntersectionObserver can detect a sentinel near the viewport for lazy work or infinite loading. Prevent duplicate loads and disconnect on teardown. ResizeObserver tracks element size changes; avoid a callback that repeatedly changes the observed size and creates a feedback loop. MutationObserver watches DOM changes, but should not be the default mechanism for synchronizing application state.

A Web Worker can run CPU-heavy JavaScript off the main thread without direct DOM access. Messages are cloned or transferred according to the data involved. Measure serialization overhead and remember workers do not make network requests inherently faster.

A service worker can intercept requests and support offline behavior. It has a lifecycle and cache-update problem of its own. Public assets and user-specific API responses need different strategies. Do not cache every authenticated response by default. PWA manifests, installability, notification permission, and service workers are related capabilities, not one indivisible feature.

## Browser-specific failure modes

Learn focus/selection behavior, scroll restoration, file input restrictions, clipboard permissions, visibility changes, and back/forward cache implications. A page becoming hidden or frozen can change timers and connection behavior. Clean up listeners, timers, observers, and subscriptions when their owner finishes.

Do not assume browser globals exist in SSR. Even code inside a framework component can run on the server. Access browser-only APIs at an appropriate client lifecycle boundary and preserve compatible initial markup.

## Practice and questions

Build a task list using delegation, persistence, and a visibility-aware refresh. Add a sentinel for loading another page and prove it does not fetch the same cursor twice. Simulate a storage failure and show a useful fallback.

**Worker or service worker?** A worker serves background computation; a service worker is event-driven infrastructure between an app and network/cache, with its own lifecycle. **Why not localStorage for all state?** It is synchronous, string-based, script-readable, and not automatically correct across users, tabs, or versions.

Exit gate: demonstrate delegated events, cleanup, one observer, and a justified storage choice.
