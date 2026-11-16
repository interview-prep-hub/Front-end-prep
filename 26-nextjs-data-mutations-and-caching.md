# 26. Next.js data, mutations, and caching

Previous: [25. App Router](25-nextjs-app-router.md) | Next: [27. Next.js production](27-nextjs-production-and-pages-router.md)

## Begin every cache answer with configuration

State the installed Next.js major and whether Cache Components are enabled. Current docs distinguish a Cache Components model from the previous model. Do not combine defaults from unrelated versions or say that every fetch is automatically cached.

In the current Cache Components model, `'use cache'`, explicit cache lifetimes, and tags describe reusable work. Current configuration also discusses Partial Prefetching; use the [caching guide](https://nextjs.org/docs/app/getting-started/caching) for the exact setup. The [previous-model guide](https://nextjs.org/docs/app/guides/caching-without-cache-components) separately documents `fetch` options and older caching patterns, including its uncached fetch default.

Illustration for an app configured with the current Cache Components model, using a public-data-only service:

```ts
import { cacheLife, cacheTag } from 'next/cache';
import { readPublicHouses } from './houses.server';

export async function getPublicHouses(city: string) {
  'use cache';
  cacheLife('minutes');
  cacheTag('public-houses');
  return readPublicHouses(city);
}
```

Keep the implementation server-only and return only approved public fields. Arguments contribute to the cache identity under the framework's rules. Choose an explicit lifetime and invalidation policy. Do not read request cookies inside a shared cached function or accidentally include user-specific output in a public cache. See the [use cache reference](https://nextjs.org/docs/app/api-reference/directives/use-cache).

## Separate cache layers

Request memoization deduplicates eligible work within a rendering/request scope. A server data/function cache can reuse work across requests. A route/render cache stores reusable rendered output under the applicable model. The browser router cache retains navigation state/output. HTTP/CDN caches follow response policy. A client query cache is another separate layer.

These layers differ in key, lifetime, scope, and invalidation. For any stale UI, identify which layer supplied the value. A router refresh does not universally purge server data caches. Database freshness alone does not prove the screen is fresh.

## Mutations and revalidation

Server Actions are reachable server operations, not inherently private functions. Validate all submitted values, authenticate the request, enforce record/tenant permissions, perform the mutation, then reconcile caches and navigation. Keep expected validation errors as useful result state instead of turning every failure into an opaque route crash. See [mutating data](https://nextjs.org/docs/app/getting-started/mutating-data).

Illustrative action module; imported services are application-owned and enforce the stated permission contract:

```ts
'use server';
import { z } from 'zod';
import { updateTag } from 'next/cache';
import { requireSession } from '@/lib/session.server';
import { completeTaskForMember } from '@/lib/tasks.server';

const inputSchema = z.object({ orgId: z.string().min(1), taskId: z.string().min(1) });
export async function completeTask(form: FormData) {
  const input = inputSchema.safeParse({
    orgId: form.get('orgId'), taskId: form.get('taskId'),
  });
  if (!input.success) return { ok: false, message: 'Invalid task information' };
  const session = await requireSession();
  await completeTaskForMember({ ...input.data, userId: session.userId });
  updateTag(`org:${input.data.orgId}:tasks`);
  return { ok: true, message: 'Task completed' };
}
```

The service must check current membership, action permission, and a task lookup scoped by both organization and task ID. The tag only matters if relevant authorized cached reads actually register it. Do not put private task reads in a shared public cache to make the example fit.

Current `updateTag` supports immediate expiry/read-your-own-writes in Server Actions. `revalidateTag` with an appropriate profile such as `'max'` supports stale-while-revalidate behavior, including from supported Route Handlers. `revalidatePath` targets a path. Know their distinct behavior and call contexts using [revalidation docs](https://nextjs.org/docs/app/getting-started/revalidating). A redirect changes navigation; it is not a substitute for invalidation.

## Fetching and endpoint choices

Fetch directly from a server service where appropriate rather than calling your own HTTP endpoint unnecessarily during server rendering. Use Route Handlers for HTTP consumers, webhooks, or externally addressable endpoints; authenticate/validate those boundaries independently. Start independent reads together and use meaningful streaming boundaries for slow regions. See [fetching data](https://nextjs.org/docs/app/getting-started/fetching-data) and [Route Handlers](https://nextjs.org/docs/app/getting-started/route-handlers).

## Practice and questions

Implement public listings with documented lifetime, an authorized task update, and a deliberate invalidation policy. Explain behavior after editing, client navigation, hard refresh, and a second browser session. Simulate a stale CDN response separately from stale client query data.

**Why is edited data still old?** Identify the actual cache and invalidation scope. **Can tags authorize data?** No. **Should private user data use the public listing cache?** No; establish authorized request handling and correct privacy scope first.

Exit gate: describe each cache layer, show one mutation becoming visible, and prove another tenant's data is not reused.
