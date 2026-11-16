# 18. TanStack Query and server state

Previous: [17. Routing](17-client-routing-and-url-state.md) | Next: [19. Forms](19-forms-validation-and-file-uploads.md)

## Queries represent identified remote data

Server state has a remote owner, freshness constraints, failure modes, and a cache lifetime. A query key identifies the data. Every variable that changes the result belongs in the key, including tenant, resource, filters, sorting, and page where applicable. A key is not a security check; the server still authorizes the request.

Focused Query v5-style example, assuming a configured `QueryClientProvider` and runtime-validated API adapter:

```tsx
import { queryOptions, useQuery } from '@tanstack/react-query';
import { listHouses } from './houses-api';

export function housesOptions(orgId: string, city: string, page: number) {
  return queryOptions({
    queryKey: ['org', orgId, 'houses', { city, page }],
    queryFn: ({ signal }) => listHouses({ orgId, city, page, signal }),
    staleTime: 30_000,
    retry: 1,
  });
}

export function Houses({ orgId }: { orgId: string }) {
  const query = useQuery(housesOptions(orgId, '', 1));
  if (query.isPending) return <p role="status">Loading houses...</p>;
  if (query.isError) return <p role="alert">Could not load houses.</p>;
  return <section aria-label="Houses">
    {query.isFetching && <p role="status">Refreshing...</p>}
    <ul>{query.data.map(house => <li key={house.id}>{house.title}</li>)}</ul>
  </section>;
}
```

The API adapter must throw for unsuccessful HTTP responses and reject invalid data. The sample assumes a nonempty authorized organization ID, an array response, and no background-error policy beyond the simple error branch. Production UI can retain previously loaded data while reporting a failed refresh.

## Freshness, retention, and retries

`staleTime` describes freshness; `gcTime` describes how long inactive cached data is retained. A fresh query can still be manually refetched. Stale does not mean deleted. Default stale queries can refetch on mount, focus, and reconnect. Query v5 defaults also include inactive retention and retries, with server/client differences; understand rather than blindly inherit them. See [important defaults](https://tanstack.com/query/latest/docs/framework/react/guides/important-defaults).

Keep initial loading, background fetching, empty success, and error distinct. Use `enabled` or another supported dependency mechanism for truly dependent requests, not to hide unclear ownership. Pagination can retain previous visible data while the next page loads, but make that placeholder behavior explicit so users do not mistake old data for the new page.

## Mutations and consistency

After a mutation, update from a canonical response or invalidate relevant keys. Return/await invalidation when the pending experience should include reconciliation. Do not invalidate unrelated tenants. A mutation result does not automatically update every query that could contain the entity.

For an optimistic cache update: cancel relevant in-flight reads, capture the previous data, apply an immutable prediction, roll back on failure when safe, and reconcile with the server. Multiple concurrent mutations can make a single-snapshot rollback incorrect, so serialize per entity, use mutation identifiers, or design a robust reconciliation rule. The [optimistic update guide](https://tanstack.com/query/latest/docs/framework/react/guides/optimistic-updates) explains supported patterns.

An optimistic task checkbox may be reasonable; a rent-payment confirmation needs authoritative server/provider state. Disabling a button reduces accidental repeats but does not provide server idempotency.

## SSR and scope

Use a request-scoped QueryClient during SSR. Dehydrate only permitted data; hydrated state is visible to the browser. Choose a suitable freshness window to avoid an immediate duplicate refetch after hydration. Keep client instances stable and clear/remove private state appropriately on logout or organization changes. See [advanced SSR](https://tanstack.com/query/latest/docs/framework/react/guides/advanced-ssr).

## Practice and questions

Add city/page keys, cancellation, one detail mutation, and targeted invalidation. Test two organizations with the same record ID and prove the cache cannot visually cross-contaminate. Simulate a failed refresh while old data exists.

**Why does data refetch on tab focus?** Check staleness and configured triggers. **Does invalidation delete data?** Generally it changes freshness and can trigger refetches, not simply erase all matching results. **Why include orgId in the key?** Cache identity differs even if resource IDs match; authorization is a separate server guarantee.

Exit gate: explain query identity, freshness versus retention, mutation reconciliation, and request-scoped SSR caching.
