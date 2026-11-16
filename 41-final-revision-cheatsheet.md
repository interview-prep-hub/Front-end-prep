# 41. Final revision cheatsheet

Previous: [40. Mocks](40-mock-interviews-and-readiness.md) | Next: [42. Official resources](42-official-resources-and-version-policy.md)

Use this for retrieval after studying the full chapters. If a line is unclear, return to the linked guide rather than memorizing it in isolation.

## Web platform and language

- Trace network, HTML/resources, style/layout/paint, and interaction separately. A quick download can still leave expensive main-thread work.
- Native button for actions, anchor for navigation, label for inputs. A role does not implement keyboard interaction.
- CSS: inspect cascade, intrinsic sizes, containing blocks, stacking contexts, and overflow before adding overrides.
- `const` fixes a binding, not nested mutation. Spread copies one level. Objects compare by identity.
- Closures retain lexical environments. Regular `this` depends on the call; arrow `this` is lexical.
- `??` preserves valid falsy values. Defaults and optional chaining require explicit null/undefined reasoning.
- Await suspends a continuation, not the main thread. Promise.all does not cancel siblings. Check response.ok.
- Debounce start frequency; separately handle response races. Clean up timers/listeners/observers.
- TypeScript checks types, not runtime JSON. Use unknown, validation, narrowing, and explicit valid states.
- Coding: clarify contract, choose a pattern, state the invariant, test edges, then analyze time/space.

Review [02](02-web-browser-and-http.md) through [11](11-typescript-for-front-end.md).

## React and application state

- Render calculates; commit applies. Keep render/updaters/reducers pure.
- State is a render snapshot. Use functional updates when depending on previous pending state.
- Stable type/position/key preserves identity. Random keys remount; index keys can confuse reordered state.
- Derive values when possible. Decide who owns drafts and when they reset.
- Effects synchronize external systems; include dependencies and pair setup with cleanup.
- Ref writes do not trigger rendering. Memoization is optimization, not correctness.
- Suspense needs supported suspending work. Error boundaries do not replace ordinary async/action error handling.
- Transition priorities differ from debounce delays. Heavy synchronous work can still block.
- URL owns shareable filters. Query owns identified remote data. Local/form state owns temporary editing.
- Query keys include every response-changing scope/input. staleTime is freshness; gcTime is inactive retention.
- Mutations need canonical reconciliation and concurrency policy. A pending label is not a confirmed payment.

Review [13](13-react-rendering-and-components.md) through [20](20-api-contracts-and-real-time-ui.md).

## Production and security

- Test behavior at the cheapest meaningful layer. Use a real browser for platform/focus/navigation behavior.
- Measure before optimizing; separate field and lab evidence. Know LCP, INP, and CLS.
- Authentication is identity; authorization is permission. Server checks must include the relevant tenant/resource.
- CORS is not auth. HttpOnly does not eliminate XSS. Client validation does not eliminate server validation.
- SSR data returned to the browser is visible. Private module-level caches can leak between server requests.
- Handle empty, loading, background refresh, error, partial failure, cancellation, and expired session deliberately.
- Observe releases and preserve a rollback/compatibility plan. Do not log tokens or sensitive form values.

Review [21](21-front-end-testing.md) through [24](24-rendering-strategies-and-hydration.md), plus [35](35-git-ci-deployment-and-observability.md).

## Framework comparison

| Concern | React + Vite SPA | Next.js App Router | TanStack Start |
| --- | --- | --- | --- |
| Core role | UI plus build tooling, with app-selected infrastructure | Full-stack React framework with App Router/RSC composition | Full-stack framework built on typed TanStack Router |
| Routing | Add a router | File conventions under app | Router conventions and generated/typed routes |
| Data | External API/adapters and chosen cache | Server services, client cache, actions/handlers | Loaders, server functions/routes, optional Query integration |
| Server boundary | Separate service unless explicitly configured otherwise | Trusted server services/actions/handlers | Supported server function/route execution boundary |
| Cache reasoning | HTTP plus client library/hosting choices | State version/config and active cache model | Separate Router, Query, HTTP, and app/server policies |
| Hosting | Static frontend plus needed APIs | Depends on server/static features | Runtime/adapter must support chosen server features |

This comparison describes the chosen practice SPA, not a claim that Vite itself cannot support SSR. Recheck APIs and current framework capabilities in 42. Be able to explain the actual app configuration, not only the row labels.

## Before the interview

Prepare your introduction, two project deep dives, one bug story, and one disagreement/learning story. Verify the call/editor environment and practice narrating a small change. Ask about input and constraints before coding. State uncertainty precisely and describe how you would verify it. Do not invent metrics, ownership, or implementation details.

Exit gate: explain every line you selected for review with an example, and put any remaining weak item on a concrete practice task.
