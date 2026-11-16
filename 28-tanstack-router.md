# 28. TanStack Router

Previous: [27. Next.js production](27-nextjs-production-and-pages-router.md) | Next: [29. TanStack Start](29-tanstack-start.md)

## Router is not Start

TanStack Router supplies typed routing, nested layouts, search-parameter validation, navigation, loading, and route lifecycle features. It can power a browser SPA without Start. TanStack Start adds full-stack build/runtime capabilities. TanStack Query handles remote data caching. These packages integrate but have different responsibilities.

Know file-based route generation, the generated route tree, router registration/type inference, root route context, child outlets, dynamic parameters such as `$houseId`, pathless layout conventions, and index routes. Check conventions against the installed version before renaming files. Generated route metadata should come from the configured generation process, not manual edits to hide a type error.

## Search, loader dependencies, and aborts

Illustrative route file, assuming file-based generation and an application adapter `listPublicHouses` that supports this execution environment:

```tsx
import { createFileRoute } from '@tanstack/react-router';
import { z } from 'zod';
import { listPublicHouses } from '../data/houses';

const searchSchema = z.object({
  city: z.string().max(100).catch(''),
  page: z.number().int().min(1).catch(1),
});

export const Route = createFileRoute('/houses')({
  validateSearch: searchSchema,
  loaderDeps: ({ search }) => ({ city: search.city, page: search.page }),
  loader: ({ deps, abortController }) => listPublicHouses({
    ...deps, signal: abortController.signal,
  }),
  component: HousesPage,
});

function HousesPage() {
  const houses = Route.useLoaderData();
  const { city, page } = Route.useSearch();
  return <main>
    <h1>Houses {city && `in ${city}`}</h1>
    <p>Page {page}</p>
    <ul>{houses.map(house => <li key={house.id}>{house.title}</li>)}</ul>
  </main>;
}
```

This uses the current documented direct Zod-schema integration. Some older schema versions/integrations need adapters. Router search parsing is not identical to manually reading raw URLSearchParams; validate the parsed values and choose fallback versus error intentionally. See [search parameters](https://tanstack.com/router/latest/docs/guide/search-params).

`loaderDeps` makes relevant search inputs explicit for loader identity. Avoid reading unrelated search state outside that dependency contract and expecting correct reload behavior. Use an abort signal with supporting adapters, and remember cancellation does not undo a completed server mutation.

## Lifecycle and data ownership

`beforeLoad` is useful for route context and coarse navigation checks. Loaders perform route data work. Pending/error/not-found components describe the route experience. Parent/child ordering and independent loading behavior can affect waterfalls; understand the [data-loading model](https://tanstack.com/router/latest/docs/guide/data-loading).

Router loader caching and Query caching are different layers. If Query owns server state, let loaders preload/ensure the same query definition and let components read that query. Avoid independently fetching the same resource under two unrelated key systems.

Typed links help catch incorrect route destinations and missing path/search inputs during development. They do not validate an arbitrary URL from outside the app or authorize the destination. Validate redirect targets and server resource access separately.

## Practice

Build city/sort/page URL state, a detail route, and a pathless authenticated layout. Test malformed search input, direct navigation, preloading, browser Back, loading cancellation, and an invalid ID. Add a typed link to detail and prove the same logical route parameters drive both navigation and data identity.

## Questions and answer cues

**Why validate search parameters?** External links and manual edits bypass compile-time types. **Why use loaderDeps?** It declares the inputs affecting loader results and reload/cache identity. **Does beforeLoad secure a server function?** No; the server function needs its own authoritative checks. **Router or Query cache?** Choose a clear owner; integrate route preloading with the chosen server-state cache.

Exit gate: explain URL parsing, route generation, loader identity, and loading/error behavior without conflating Router, Query, and Start.
