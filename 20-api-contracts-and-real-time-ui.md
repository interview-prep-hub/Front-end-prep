# 20. API contracts and real-time UI

Previous: [19. Forms](19-forms-validation-and-file-uploads.md) | Next: [21. Testing](21-front-end-testing.md)

## Design the contract before the component

Agree on resource IDs, tenant context, field names, nullability, date/time format, money representation, pagination, filtering, sorting, error format, rate limits, and version compatibility. Map transport objects into stable UI/domain types when the backend shape is inconvenient or legacy-specific.

Focused boundary example, with the application-owned schema imported explicitly:

```ts
import { houseListSchema } from './house-schema';

export async function getHouses(orgId: string, signal?: AbortSignal) {
  const response = await fetch(`/api/orgs/${encodeURIComponent(orgId)}/houses`, {
    credentials: 'same-origin', signal,
    headers: { Accept: 'application/json' },
  });
  if (!response.ok) throw new Error(`Could not load houses (${response.status})`);
  return houseListSchema.parse(await response.json());
}
```

This read example intentionally does not define mutation CSRF behavior, refresh tokens, or application-wide error reporting. Centralize those policies in a deliberate adapter rather than copying an incomplete wrapper everywhere. A 204 response has no JSON body to parse.

## REST, GraphQL, and RPC

REST-oriented APIs expose resource operations through HTTP conventions. GraphQL lets callers select typed fields with arguments, variables, fragments, queries, and mutations. RPC models callable operations and can provide useful end-to-end types in a TypeScript stack. None automatically implements correct permissions or cache policy.

For GraphQL, distinguish transport errors from a response containing GraphQL errors and possibly partial data. Explain normalized caching if the chosen client uses it, field/operation identity, pagination, and the risk of overfetching expensive relationships. Use variables rather than concatenating user input into query text. See [GraphQL queries](https://graphql.org/learn/queries/).

For offset pagination, explain shifting data and page-number UX. Cursor pagination suits many changing ordered feeds but requires a deterministic order/tie-breaker and a clear cursor contract. Do not assume a cursor is an authorization token.

## Real-time options

| Mechanism | Good fit | Tradeoffs |
| --- | --- | --- |
| Periodic polling | Simple job progress or low-frequency updates | Latency and repeated requests; pause/slow when appropriate |
| SSE | Server-to-client event stream over HTTP | Connection/proxy behavior, auth model, reconnection, event IDs |
| WebSocket | Bidirectional interactive communication | Heartbeats, reconnect, auth renewal, ordering, backpressure |
| Push notification | Re-engagement outside the active app | Permission and delivery are not a reliable data source |

The browser EventSource API supports SSE's streaming/reconnection model; verify credentials and infrastructure behavior for your deployment using [MDN's SSE guide](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events).

After reconnect, reconcile with authoritative state or resume from a supported event ID. Handle duplicate, missing, and out-of-order events. Invalidate a query or apply a versioned update rather than blindly appending every message. Scope subscriptions by user and organization, and tear them down on scope changes.

## Resume practice: background inventory sync

Design a screen showing pending, running, succeeded, partially failed, and failed sync jobs. Show last successful sync time separately from the current job's status. An accepted job request is not completed work. Support retry only for permitted operations and explain whether the server retries individual items or the whole job.

For pricing, show stale indicators and timestamps when appropriate. An external market price is not automatically the same as a store's sale price. Avoid silently overwriting a user's newer edit with an older sync result; use versions, explicit policy, or conflict UI.

## Questions and answer cues

**When should a request retry?** Consider method semantics, idempotency, failure type, backoff, Retry-After, and cancellation. **Why not WebSockets for everything?** Simpler transport may meet latency needs with less operational cost. **How do you handle a breaking Rails API change?** Use an adapter, compatibility period, contract tests, and staged rollout.

Exit gate: define a typed contract, distinguish HTTP/parse/domain errors, and design reconnect plus stale-data behavior for one real-time feature.
