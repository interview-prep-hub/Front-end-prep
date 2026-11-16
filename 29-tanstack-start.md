# 29. TanStack Start

Previous: [28. Router](28-tanstack-router.md) | Next: [30. Start data, auth, oRPC](30-tanstack-start-data-auth-and-orpc.md)

## Framework model

Start builds on TanStack Router with full-document SSR, streaming, server functions, server routes, and client/server build support. Current docs describe stable Start with Vite and Rsbuild support, while React Server Components are an experimental feature. Do not describe ordinary SSR or a server function as RSC. See the [Start overview](https://tanstack.com/start/latest/docs/framework/react/overview).

Use the current local scaffold command from the [getting-started guide](https://tanstack.com/start/latest/docs/framework/react/getting-started):

```powershell
npx @tanstack/cli@latest create
```

Choose a new practice directory such as `practice-start` and the desired React/Start options. Follow the generated scripts and runtime requirements. The CLI and templates change, so do not assume an old scaffold from a blog still matches the current package.

## Build and document boundaries

With Vite, the Start plugin precedes the React plugin in the documented configuration. The router factory and root document shell connect generated routes, head content, scripts, and rendering. Keep these framework pieces intact when adding providers. See [building from scratch](https://tanstack.com/start/latest/docs/framework/react/build-from-scratch).

Route code and loaders can participate in both server rendering and client navigation. A normal loader is not automatically a server-only security boundary. Keep database credentials, private SDKs, and trusted session logic in server-only code reached through an appropriate server function/route.

## Server function example

Focused current API example. The two imports from local server modules are application-owned services, not Start exports:

```ts
import { createServerFn } from '@tanstack/react-start';
import { z } from 'zod';
import { requireSession } from './session.server';
import { readHouseForMember } from './houses.server';

export const getHouse = createServerFn({ method: 'GET' })
  .validator(z.object({ orgId: z.string().min(1), houseId: z.string().min(1) }))
  .handler(async ({ data }) => {
    const session = await requireSession();
    return readHouseForMember({ ...data, userId: session.userId });
  });

// From a supported caller: await getHouse({ data: { orgId, houseId } });
```

`readHouseForMember` must check current organization membership and lookup the record within that organization before returning minimal permitted fields. Use POST for mutations. A typed parameter annotation alone is not runtime validation; this sample uses a schema. Current docs use `.validator`; older projects can have a different API spelling, so match the installed package. See [server functions](https://tanstack.com/start/latest/docs/framework/react/guide/server-functions).

The build transforms supported server-function calls across client/server boundaries. It does not make every imported function server-only by intention alone. Follow import-protection and execution rules, inspect emitted client assets, and avoid leaking private data through returned values. Inputs/outputs must fit the framework's transport capabilities.

## Server routes, errors, and hosting

Use server routes for ordinary HTTP consumers, webhooks, or responses needing direct HTTP behavior. Use server functions for appropriate framework-connected calls. Both require validation, auth, permission checks, and sensible response/error handling.

Handle expected validation failures as useful application results. Let supported redirects/not-found behavior flow through the framework without catching it as a generic failure. Log enough context for diagnosis without exposing secrets in client errors.

Choose hosting based on the current supported adapter/build output, runtime APIs, environment handling, response streaming, and operational constraints. Do not assume a plain static SPA host can execute server functions. Avoid naming an old server engine as a timeless property of Start.

## Practice and questions

Build a public SSR listing, a protected read server function, a POST mutation, a loading/error state, and an invalid-resource response. Inspect both initial HTML and client navigation. Confirm that no database key appears in browser assets.

**Can a loader call the database directly?** Only through code whose execution and import boundary really keep it server-side; use the supported server boundary. **Why a server function instead of fetch?** Typed framework integration can be convenient, while HTTP routes remain useful for broader consumers. **Does type safety validate a forged request?** No, runtime validation and permission checks still apply.

Exit gate: trace a request from route to server function to authorized service and back, including a subsequent client navigation.
