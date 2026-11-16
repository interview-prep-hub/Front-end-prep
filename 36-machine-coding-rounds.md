# 36. Machine-coding rounds

Previous: [35. Delivery](35-git-ci-deployment-and-observability.md) | Next: [37. Capstones](37-capstone-projects.md)

## Work in observable slices

For a 60-minute round, spend 5 minutes clarifying requirements, 5 outlining state/data and a component tree, 30 building the core flow, 10 handling important failures/accessibility, and 10 testing and explaining tradeoffs. If the interviewer gives 30 minutes, reduce scope explicitly instead of silently abandoning requirements.

Narrate decisions without reading every line aloud. State assumptions, make a working vertical slice early, and verify it before adding polish. Use plain CSS unless a library is allowed and useful. Do not spend the first half installing a large UI kit.

## Round specifications

| Round | Core acceptance | Follow-up |
| --- | --- | --- |
| Autocomplete, 60 min | Labeled input, debounce, loading/empty/error, keyboard selection, latest query wins | Cache results, clear input, invalidate selection |
| Data table, 60 min | Stable row IDs, sort, filter, paging, empty state | Selection across pages, server paging, virtualization |
| Nested explorer, 45 min | Expand/collapse, stable IDs, visible selection | Search paths, deep trees, cycle handling |
| Resident tasks, 60 min | Add/toggle/delete, immutable state, validation | Optimistic failure, undo, multi-user conflicts |
| Housing application, 60 min | Fields, client/server errors, pending submit, preserve input | Autosave and file preview cleanup |
| Modal/tabs, 45 min | Correct labels, focus, keyboard/dismissal, controlled state | Nested overlays or dynamic tab removal |
| Infinite feed, 60 min | Cursor loading, dedupe, load guard, retry | Cancellation, scroll restoration, accessibly reachable footer |
| Shopping cart, 60 min | Quantity bounds, totals, removal, empty state | Discounts, currency rules, stock revalidation |

For each round, write a contract, create a minimal fixture/API simulator, implement, then test one normal and two unhappy paths. Keep a short review: requirement missed, root cause, better state model, next retry date.

## Worked utility: trailing debounce with cancellation

Contract: run the most recent call after `wait` milliseconds of inactivity; preserve its receiver/arguments; allow pending work to be canceled. This version does not promise a meaningful synchronous return value, leading calls, max-wait, or promise aggregation.

```js
function debounce(fn, wait) {
  let timer;
  function debounced(...args) {
    clearTimeout(timer);
    const receiver = this;
    timer = setTimeout(() => {
      timer = undefined;
      fn.apply(receiver, args);
    }, wait);
  }
  debounced.cancel = () => {
    clearTimeout(timer);
    timer = undefined;
  };
  return debounced;
}
```

Debouncing controls when work starts; it does not stop a previously started search response from winning. Combine it with request identity/cancellation. In React, keep the debounced function's lifetime deliberate and cancel it during cleanup. Recreating it on every keystroke defeats its shared timer.

Throttle instead limits execution frequency during continuous events. Decide leading/trailing behavior explicitly. For visual scroll work, consider frame scheduling rather than assuming a fixed millisecond interval matches the screen.

## Worked utility: bounded async concurrency

Contract: return results in input order, run at most `limit` mapper operations concurrently, reject on an operation failure. Like Promise.all, rejection does not cancel already running work. This basic version may continue other workers after a failure; fail-fast scheduling/cancellation would be an additional contract.

```js
async function mapLimit(items, limit, mapper) {
  if (!Number.isInteger(limit) || limit < 1) {
    throw new RangeError('limit must be a positive integer');
  }
  const results = new Array(items.length);
  let next = 0;
  async function worker() {
    while (next < items.length) {
      const index = next++;
      results[index] = await mapper(items[index], index);
    }
  }
  const count = Math.min(limit, items.length);
  await Promise.all(Array.from({ length: count }, () => worker()));
  return results;
}
```

The synchronous increment assigns each index before a worker awaits. State that assumption when discussing execution. For a production upload queue, add per-item status, cancellation, retry/idempotency, and bounded memory for very large inputs.

## Testing checklist and narration

For debounce, verify one delayed call, replacement by a newer call, preserved arguments, cancel, and teardown. For mapLimit, verify empty input, invalid limits, output order under different delays, max active count, and rejection. For a UI, check keyboard operation, stale results, error recovery, and unmount while work is pending.

If time expires, explain the current behavior, the remaining requirement, and the next concrete change. Do not describe an unfinished mock as fully implemented. Exit gate: complete two different rounds under time pressure and repeat each after reviewing the failure notes.
