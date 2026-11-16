# 22. Performance and debugging

Previous: [21. Testing](21-front-end-testing.md) | Next: [23. Security](23-security-authentication-and-multi-tenancy.md)

## Measure the user's symptom

Define the slow action, environment, device/network, and success criterion before optimizing. Distinguish slow response time, download, parsing/execution, rendering, layout, paint, hydration, and interaction latency. Compare equivalent production builds and realistic devices. Development Strict Mode and instrumentation can change measurements.

Core Web Vitals currently include LCP, INP, and CLS. Good thresholds are LCP at most 2.5 seconds, INP at most 200 milliseconds, and CLS at most 0.1, assessed at the 75th percentile of page visits. Field data describes actual users; lab data supports controlled diagnosis. See [web.dev's Web Vitals reference](https://web.dev/articles/vitals).

| Symptom | Evidence to collect | Candidate fix |
| --- | --- | --- |
| Slow first content | TTFB, waterfall, critical resources | Reduce server work, cache eligible content, remove waterfalls |
| Slow largest content | LCP element, image discovery/size | Correct image dimensions/format, earlier loading, avoid critical lazy loading |
| Input lag | Long tasks, React commits, handler cost | Reduce main-thread work, narrow updates, virtualize, defer suitable work |
| Page jumps | Layout-shift entries | Reserve space, stabilize fonts/ads/async regions |
| Large startup bundle | Bundle analysis and coverage | Route splitting, remove heavy dependencies, reduce client boundaries |
| Memory grows over navigation | Heap snapshots and retained objects | Teardown listeners/subscriptions, bound caches |

## React-specific reasoning

Use React DevTools Profiler to find expensive commits and why components render. First fix state location, unstable keys, excessive context updates, expensive calculations, and unnecessary effects. Then consider memoization where measurements justify it.

`memo` compares props under its comparison rules. New object/function props can defeat a useful memo boundary. `useMemo` and `useCallback` have bookkeeping costs and should not become correctness dependencies. A compiler-enabled app may automate some memoization, but data ownership and heavy algorithms still need attention.

Virtualization reduces mounted rows, but introduces variable-height, focus, screen-reader, print, and search concerns. Pagination may satisfy the user with less complexity. Infinite scrolling needs a recovery path, loading guard, and understandable navigation.

## Browser and network work

Batch style/DOM reads and writes to avoid forced layout loops. Use suitable animation properties and profile actual layers. Debounce network-driven searches where useful; throttle or schedule high-frequency visual work appropriately. Move heavy pure computation to a worker only when transfer and coordination cost make sense.

Optimize images with appropriate source sizes, formats, dimensions, and loading priority. Reduce unnecessary font variants and blocking work. Use resource hints only after identifying the critical bottleneck. HTTP caching and compression help delivery but do not reduce the execution cost of unused JavaScript after it is loaded.

## A debugging method

1. Reproduce with the smallest reliable steps and record expected versus actual behavior.
2. Identify the failing layer with console, network, state, and timing evidence.
3. Form one hypothesis and change one relevant variable.
4. Verify the fix under the original failing condition and a normal case.
5. Add a regression test when it captures a meaningful failure; record the cause and evidence.

Avoid random dependency upgrades as the first debugging move. Check recent changes, configuration, response shapes, stale assets, clock/time-zone assumptions, and environment differences.

## Practice and questions

Create a deliberately slow searchable table. Measure typing latency, identify whether filtering or row rendering dominates, and compare one algorithmic fix with one rendering fix. Record before/after data and the environment. Do not claim a resume improvement without measurements from the actual work.

**Why did memoization not improve LCP?** The bottleneck may be the server, image, or network rather than rerenders. **Why can Lighthouse look good while users complain?** Lab conditions may miss devices, long sessions, interaction patterns, and field variability. **Why not lazy-load every component?** Extra requests and delayed critical content can make the experience worse.

Exit gate: explain one measured improvement with a trace and identify the tradeoff it introduced.
