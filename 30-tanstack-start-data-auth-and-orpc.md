# 30. TanStack Start data, authentication, and oRPC

Previous: [29. Start](29-tanstack-start.md) | Next: [31. Rails and migration](31-react-on-rails-hotwire-and-migration.md)

## Integrate route loading with Query deliberately

Create the router and QueryClient with the correct lifetime: a new instance for each SSR request and a stable router-associated client in the browser. A process-wide private QueryClient can reuse one user's data in another response.

The current documented integration uses `@tanstack/react-router-ssr-query`. This is a focused router setup; the generated route tree and root context typing must exist in the practice app:

```tsx
import { QueryClient } from '@tanstack/react-query';
import { createRouter } from '@tanstack/react-router';
import { setupRouterSsrQueryIntegration } from '@tanstack/react-router-ssr-query';
import { routeTree } from './routeTree.gen';

export function getRouter() {
  const queryClient = new QueryClient({
    defaultOptions: { queries: { staleTime: 30_000 } },
  });
  const router = createRouter({
    routeTree,
    context: { queryClient },
    defaultPreload: 'intent',
    defaultPreloadStaleTime: 0,
  });
  setupRouterSsrQueryIntegration({ router, queryClient });
  return router;
}
```

The integration manages its provider and SSR transfer. Do not introduce a second unrelated client/provider or duplicate manual dehydration without understanding the integration contract. Define `queryClient` in root route context types. See [Start with TanStack Query](https://tanstack.com/start/latest/docs/framework/react/guide/tanstack-query).

Create reusable query options whose query function calls a validated server function or server-safe API adapter. Use them in route preloading and component reads. A relative browser fetch wrapper is not automatically usable in a Node SSR environment. For private queries, include the relevant user/tenant/resource identity and serialize only authorized fields.

## Session and tenant lifecycle

Load session state at a trusted server boundary and provide minimal route context for navigation UX. A `beforeLoad` redirect is helpful, but each private server operation still enforces session and resource permissions. Middleware can centralize repeated request context or checks, while resource-specific decisions remain near the operation. See [Start authentication](https://tanstack.com/start/latest/docs/framework/react/guide/authentication) and [middleware](https://tanstack.com/start/latest/docs/framework/react/guide/middleware).

After login/logout, invalidate route session context as appropriate and clear private query data. After organization switching, cancel obsolete work, update context and keys, unsubscribe old real-time streams, and reset organization-specific drafts. Prevent late results from replacing the new organization's screen.

If the root document includes session-specific data, use a private response/cache policy that remains private through the CDN. Typed route context is not proof that a session remains valid during a later mutation.

## Where oRPC fits

oRPC provides typed procedures and client calls; Start provides application routing/rendering/runtime boundaries. Use a deliberate RPC/API layer when it benefits shared web/mobile consumers or consistent procedure contracts. Do not wrap the same operation in multiple redundant transport layers without a reason. The [oRPC getting-started guide](https://orpc.dev/docs/getting-started) describes procedures, routers, schemas, and handlers for its current major.

Conceptual operation contract, not library-specific syntax:

```text
completeTask(input: { orgId, taskId, expectedVersion })
  authenticated context supplies userId
  input schema validates values
  membership and permission check authorizes org/task
  versioned update detects conflicting edits
  response supplies canonical task or a typed conflict result
```

Drizzle belongs behind the trusted data-access boundary. In front-end interviews, explain how its server queries support the contract and tenant filters; you do not need to expose the ORM to the browser. Shared types do not remove migrations, runtime validation, authorization, or compatibility concerns.

For Rails sync APIs during migration, define the source of truth for each field, ID mapping, versioning, retries, duplicate delivery, and reconciliation. UI should show pending versus synchronized state honestly. A new TypeScript platform does not automatically make legacy data consistent.

## Practice and questions

Build an authorized task query and mutation with one shared query definition. Render on the server, navigate on the client, mutate, invalidate, and switch organizations while a request is pending. Add direct-call tests for another organization's task.

**Why both route preload and useQuery?** Preload starts/populates the same cache before rendering; the component observes it. **Does router.invalidate clear all private query data?** Not automatically as a universal policy; handle both owners deliberately. **Why oRPC with Start?** Explain shared contracts/consumers or middleware conventions, not simply because both are typed.

Exit gate: demonstrate one SSR request lifecycle and prove session, query, and tenant scope stay correct through login, logout, mutation, and navigation.
