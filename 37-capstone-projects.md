# 37. Capstone projects

Previous: [36. Machine coding](36-machine-coding-rounds.md) | Next: [38. Resume and behavioral](38-resume-deep-dives-and-behavioral.md)

## Choose one domain and finish a reviewable slice

Build one primary capstone rather than three unfinished large applications. Use synthetic data and your own code. The projects below are practice specifications connected to your experience, not replicas of private employer systems or proof of past ownership.

Recommended: a housing marketplace with an organization-scoped resident task area. Keep billing simulated. The same small feature can then be implemented in Vite, Next.js, and Start to compare architecture without rebuilding everything.

## Shared requirements and contract

Public pages: listing search and detail. Private pages: organization-scoped task list and task update. All implementations need labeled controls, responsive layout, loading/empty/error states, URL filters, request race handling, and a recovery path.

```text
HouseSummary: { id, title, city, monthlyRentMinor, currency, thumbnailUrl }
HouseDetail: summary + { description, amenities, availabilityVersion }
Task: { id, orgId, title, done, version }
List houses: validated filters + deterministic page/cursor contract
Complete task: { orgId, taskId, expectedVersion } -> canonical task or conflict
```

Choose exact HTTP/RPC shapes in your implementation notes. IDs and tenant permissions need server validation. Use a real server/test boundary for authorization claims; a browser-only mock can demonstrate UI states but cannot prove server security.

## Milestones and evidence

| Milestone | Deliverable | Acceptance evidence |
| --- | --- | --- |
| 1. Contract and static UI | Fixtures, route map, accessible page skeleton | Keyboard flow and narrow viewport screenshots |
| 2. Search | URL filters, sorting, paging, request adapter | Reload/share/Back works; old response cannot replace new results |
| 3. Detail and form | Detail route, validated application draft | Invalid ID, field errors, failed submit preserve input |
| 4. Private tasks | Session boundary, org-scoped reads/writes | Different-org direct request denied; read-only write denied |
| 5. Consistency | Query keys, mutation reconciliation, version conflict | Delayed response, failed update, organization switch tests |
| 6. Quality and release | Meaningful tests, production build, basic monitoring plan | Test output, build record, measured performance trace |
| 7. Interview package | Architecture note and five-minute demo | Requirements, tradeoffs, one bug/fix, and limitations explained |

Finish each milestone before adding maps, chat, payments, or advanced animations. A small finished feature gives stronger interview evidence than many navigation links to incomplete screens.

## Implement the comparison slice in three stacks

**Vite SPA:** use a router and Query, call a separate API, configure deep-link hosting, and document client environment exposure. Explain startup/data loading, server auth ownership, and why this deployment fits the chosen private/public requirements.

**Next.js App Router:** render public detail content through a server service, isolate interactive client components, add one authorized Server Action or Route Handler, and document the installed caching model. Verify metadata, not-found, and revalidation behavior.

**TanStack Start:** validate route search/loader inputs, use a schema-validated server function, integrate one request-scoped QueryClient with the current SSR helper, and verify initial HTML plus client navigation. Keep authorization in the server operation.

Port only listing read and task update for the comparison if time is short. Hold fixtures and acceptance cases constant. Record differences in code ownership, loading/caching, network calls, client bundle, hosting, and developer complexity. Do not conclude a framework is universally faster from one tiny benchmark.

## Alternative domains

Inventory sync dashboard: catalog search, stale price badges, paginated jobs, partial failures, retry policy, and bulk-edit conflict handling. This supports API/background-job discussions linked to CrystalCommerce.

Learning platform: catalog, role-aware dashboard, quiz draft/autosave, authoritative submit, and simulated subscription status. This supports LMS stories without integrating real payments or requiring external accounts.

## Review rubric

Assess correctness, accessibility, type/runtime boundaries, state/cache ownership, failure recovery, meaningful tests, measured performance, and explanatory clarity. Add a limitations section describing incomplete or intentionally simulated behavior. Keep a repeatable runbook with versions, scripts, fixture setup, and how to trigger a failure.

Exit gate: demo a finished flow, prove two failures recover correctly, explain one rejected alternative, and show evidence for every performance/security claim you make.
