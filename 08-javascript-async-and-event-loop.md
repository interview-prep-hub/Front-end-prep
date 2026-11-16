# 08. Async JavaScript and the event loop

Previous: [07. Functions and memory](07-javascript-functions-objects-and-memory.md) | Next: [09. Browser APIs](09-dom-and-browser-apis.md)

## Scheduling model

Synchronous code runs on the current call stack. Browser tasks include callbacks such as timers and many events. Promise reactions and `queueMicrotask` callbacks use the microtask queue. At a microtask checkpoint, queued microtasks run until drained, including newly queued microtasks. Rendering opportunities occur according to browser scheduling, not after every individual callback. See [MDN's microtask guide](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide).

```js
console.log('A');
setTimeout(() => console.log('B'), 0);
Promise.resolve().then(() => console.log('C'));
queueMicrotask(() => console.log('D'));
console.log('E');
// In a browser: A, E, C, D, B
```

A zero-delay timeout is not immediate. A long synchronous loop still blocks input and paint. An endless chain of microtasks can also starve other work. `requestAnimationFrame` aligns visual updates with a rendering opportunity; it is not a substitute for moving CPU-heavy work off the main thread.

## Promises and async functions

A Promise executor runs immediately. `.then` handlers run asynchronously. An async function returns a promise, and `await` suspends that function's continuation, not the whole thread. A synchronous throw inside an async function becomes a rejection of its returned promise.

Use `Promise.all` for required independent work, `allSettled` for inspecting every result, `race` for the first settlement, and `any` for the first fulfillment. `all` rejects on a rejection, but does not cancel other operations. Catch at a layer that can recover or report meaningfully; swallowing every rejection turns failures into misleading success states.

```js
async function loadDashboard(signal) {
  const [profileResponse, tasksResponse] = await Promise.all([
    fetch('/api/profile', { signal }),
    fetch('/api/tasks', { signal }),
  ]);
  if (!profileResponse.ok || !tasksResponse.ok) {
    throw new Error('Dashboard request failed');
  }
  return Promise.all([profileResponse.json(), tasksResponse.json()]);
}
```

Do not use `forEach(async ...)` expecting the outer operation to wait. Use `for...of` for sequential work or `map` plus `Promise.all` for independent work. Use a concurrency limit when the collection could overwhelm an API or the browser.

## Cancellation and races

For search, request A may finish after request B. Associate results with the active query, abort obsolete requests, or ignore results that no longer belong to the current operation. Cancellation is not proof the server never performed a mutation. For writes, use server-side idempotency and conflict policies when required.

```js
let activeController;
let requestId = 0;
async function search(query, render, showError) {
  activeController?.abort();
  const controller = new AbortController();
  activeController = controller;
  const currentId = ++requestId;
  try {
    const response = await fetch(`/api/search?q=${encodeURIComponent(query)}`, {
      signal: controller.signal,
    });
    if (!response.ok) throw new Error(`Search failed: ${response.status}`);
    const data = await response.json();
    if (currentId === requestId) render(data);
  } catch (error) {
    if (!controller.signal.aborted && currentId === requestId) showError(error);
  }
}
```

The sample assumes UI callbacks and an agreed response schema. Add runtime validation at a real external boundary. On teardown, abort and invalidate the active request ID.

## Practice and questions

Build delayed fake requests that finish out of order. Prove that the latest query always wins. Add a retry policy that retries a transient read with a bounded backoff, respects rate limiting, and does not blindly replay a payment.

**Does await make code parallel?** No; start independent work before awaiting it. **Does Promise.all cancel on failure?** No. **Why can an async function still freeze a page?** Its synchronous portions still execute on the main thread.

Exit gate: predict scheduling, handle rejection, distinguish sequential/parallel work, and demonstrate a race fix.
