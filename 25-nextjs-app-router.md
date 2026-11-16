# 25. Next.js App Router

Previous: [24. Rendering](24-rendering-strategies-and-hydration.md) | Next: [26. Next.js data and caching](26-nextjs-data-mutations-and-caching.md)

## Setup and route structure

For a new practice app, run `npx create-next-app@latest practice-next` from this study directory and choose TypeScript and App Router. Inspect the generated config, scripts, and dependency versions. The [installation guide](https://nextjs.org/docs/app/getting-started/installation) is the reference for changing defaults and runtime requirements.

```text
app/
  layout.tsx
  page.tsx
  houses/
    page.tsx
    loading.tsx
    [houseId]/
      page.tsx
      not-found.tsx
  dashboard/
    layout.tsx
    page.tsx
    error.tsx
  api/
    health/
      route.ts
```

`page` exposes a route's UI, `layout` composes shared UI, `loading` supplies an appropriate loading boundary, `error` handles route-segment errors under framework rules, and `route` defines an HTTP endpoint. The root layout includes the document structure. Know dynamic segments, catch-all and optional catch-all segments, route groups, templates, and not-found handling. Parallel/intercepting routes are stretch topics with useful modal-navigation applications.

## Server and client composition

App Router pages/layouts are Server Components by default. Use Client Components for stateful browser interaction and hooks that require client execution. `'use client'` establishes a module boundary, and its imported client dependency graph contributes to client code. Keep the boundary as small as practical.

It does not mean the component never contributes to server-generated initial HTML. It also does not automatically mark its rendered children as client modules when server-rendered content is composed through props. Cross-boundary values must be supported by React/framework serialization. See [Server and Client Components](https://nextjs.org/docs/app/getting-started/server-and-client-components).

Focused route example, assuming an application-owned, server-only data service:

```tsx
import { notFound } from 'next/navigation';
import { getPublicHouse } from '@/lib/houses.server';
import { FavoriteButton } from '@/components/FavoriteButton';

type Props = { params: Promise<{ houseId: string }> };
export default async function HousePage({ params }: Props) {
  const { houseId } = await params;
  const house = await getPublicHouse(houseId);
  if (!house) notFound();
  return <main>
    <h1>{house.title}</h1>
    <p>{house.description}</p>
    <FavoriteButton houseId={house.id} />
  </main>;
}
```

The service must expose only public fields and follow a deliberate cache policy. Private resources require session/membership checks. In Next.js, a `.server.ts` filename alone is not an enforced boundary: add `import 'server-only'` to private service modules so an accidental client import is caught by the framework. `FavoriteButton` owns its client interaction and must call an authorized mutation. Current request APIs such as params, searchParams, cookies, and headers have async usage patterns; older tutorials may show synchronous access.

## Navigation, metadata, and failure states

Use framework links for supported client navigation/prefetch behavior. Distinguish pathname/search-parameter hooks from server route props. Decide which search values are validated on the server and whether editing them should push or replace history. A change in route does not necessarily remount every layout or discard all client state.

Provide meaningful metadata, canonical URLs, and social-preview content. Use static metadata or dynamic metadata generation according to page needs. Make missing data a real not-found condition rather than rendering a successful blank detail page.

Place error/loading boundaries around useful segments. A client error boundary does not replace validation messages for expected failures. Avoid catching framework redirect/not-found control flow indiscriminately. Keep secrets in server-only modules and remember that returned/rendered data is visible to the client.

## Practice and questions

Build a public listing/detail route, a shared dashboard layout, a client filter control, a loading state, and an invalid-ID not-found state. Inspect the client bundle and identify what each client boundary pulled in.

**Why not mark the whole app use client?** It moves unnecessary modules into the client graph and reduces server composition opportunities. **Can a Server Component use a database?** Through appropriately protected server code, while keeping returned data and caching safe. **What persists during navigation?** Explain layout identity, route structure, and client state rather than assuming a full page reload.

Exit gate: draw server/client boundaries and explain the initial request plus a subsequent client navigation.
