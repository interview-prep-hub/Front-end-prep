# 17. Client routing and URL state

Previous: [16. State architecture](16-state-management-and-architecture.md) | Next: [18. TanStack Query](18-tanstack-query-and-server-state.md)

## Treat the URL as a public interface

Path parameters identify resources and route structure. Search parameters describe optional filters, sort order, pagination, or other addressable state. A fragment often identifies an in-page location. Keep secrets and sensitive personal input out of URLs because URLs can appear in history, logs, analytics, and shared links.

Choose push when a change should form a navigation step; choose replace for transient typing or canonicalization that should not produce dozens of Back entries. Requirements decide the policy. Support direct loading, refresh, browser Back/Forward, copied links, and invalid URLs.

```ts
type Filters = { city: string; page: number };
export function parseFilters(search: string): Filters {
  const params = new URLSearchParams(search);
  const rawPage = params.get('page');
  const page = rawPage === null ? 1 : Number(rawPage);
  return {
    city: params.get('city')?.trim() ?? '',
    page: Number.isSafeInteger(page) && page >= 1 ? page : 1,
  };
}
```

Decide whether repeated values, excessive input, unknown parameters, and invalid pages are rejected, defaulted, or canonicalized. Changing a filter generally resets pagination. Do not let local state become a second canonical copy that fights with browser navigation.

## Router responsibilities

A route defines a component boundary and may define loading, mutations, pending UI, errors, not-found behavior, and metadata. Nested layouts allow shared UI with child outlets. A pathless layout can group UI without adding a URL segment. A lazy route can split code by navigation need.

React Router has declarative, data, and framework modes; know which mode an example assumes before mixing APIs. Its [mode guide](https://reactrouter.com/start/modes) explains the distinctions. Older workplace code may use different imports or versions. TanStack Router is covered in 28, and Next.js routing in 25.

Route guards improve navigation behavior, but they do not authorize a resource. A user can call an API or server function directly. Server-side session, tenant, permission, and record checks remain required.

## Loading, navigation, and errors

Prefetch route code or data when likely to be useful, such as on intentional hover/focus, but account for bandwidth and private data. Independent loaders should not form an unnecessary parent-child waterfall. Distinguish a route navigation indicator from a background query refresh.

Design scroll restoration and focus after navigation. Updating the URL without helping a keyboard or screen-reader user discover the new page can produce a confusing experience. Preserve legitimate native link behavior, including opening a new tab and copying a destination.

Use a real not-found state and correct server status where appropriate. For a static SPA, hosting fallback may always serve the entry document, so application not-found rendering and HTTP status behavior differ from server-routed apps. Explain that deployment boundary clearly.

## Practice

Build `/houses?city=Lahore&page=2` and `/houses/:houseId`. Change filters, navigate to a detail, use Back, reload the detail directly, and open a malformed page parameter. Preserve unrelated query parameters when changing one field. Add a route-level error with a retry/recovery path.

## Questions and answer cues

**What belongs in the URL?** Addressable, shareable, reconstructible navigation state that is safe to expose. **Why not use only local state for filters?** It loses refresh/share/back behavior. **When should a guard run?** At an appropriate navigation boundary for UX, while authoritative checks run at the resource boundary. **Why does one page load twice?** Inspect prefetch, loader cache, query cache, Strict Mode, and actual request ownership before guessing.

Exit gate: demonstrate direct-link and Back/Forward correctness, explain push versus replace, and show malformed URL handling.
