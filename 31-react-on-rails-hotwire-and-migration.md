# 31. React on Rails, Hotwire, and migration

Previous: [30. Start data and auth](30-tanstack-start-data-auth-and-orpc.md) | Next: [32. React Native and Expo](32-react-native-and-expo.md)

## Explain the integration actually used

React inside a Rails application can mean isolated interactive roots, larger application regions, or server-rendered React with hydration. React on Rails supplies integration conventions, but its capabilities and lifecycle depend on the installed edition/version and app setup. Verify the actual codebase rather than claiming every Rails/React app has the same architecture. See [React on Rails documentation](https://reactonrails.com/docs/).

Trace a real page: Rails route/controller, authorized data loading, safe serialized props, registered component, asset bundle, mount/hydration, API interaction, and unmount. Explain the boundary between ERB-owned DOM and React-owned DOM. Two renderers should not independently mutate the same subtree.

Treat server-rendered props as browser-visible. Serialize with the framework's safe helpers rather than concatenating unescaped JSON into scripts/HTML. Keep API and asset origins, session cookies, CSRF tokens, and deployment asset digests consistent.

## Hotwire concepts

Turbo Drive accelerates navigation/form flows by updating the page without the usual full reload. Turbo Frames scope selected navigations to a region. Turbo Streams describe targeted DOM updates. Stimulus attaches behavior to HTML with controllers, targets, values, actions, and lifecycle callbacks. Use the [Rails JavaScript guide](https://guides.rubyonrails.org/working_with_javascript_in_rails.html) to ground the stack's choices.

Turbo navigation changes page lifecycle assumptions. A `DOMContentLoaded`-only setup may not run on subsequent visits. Cached snapshots and restored pages can retain unexpected visual state. The [Turbo Drive handbook](https://turbo.hotwired.dev/handbook/drive) explains its navigation/cache model. Use the integration's documented mount/unmount hooks instead of piling custom listeners on top of it.

Stimulus controllers can connect and disconnect repeatedly; their listeners, timers, observers, and third-party widgets need matching cleanup. The [Stimulus lifecycle reference](https://stimulus.hotwired.dev/reference/lifecycle-callbacks) describes these callbacks. The same ownership principle applies to a React root on a page replaced by Turbo.

## Choosing React or server-driven UI

For a simple server-backed form or content update, Rails/Hotwire can avoid building another client state architecture. React can be useful for complex local workflows, reusable interactive regions, and richer client-side composition. Compare required interactions, team skills, delivery cost, testing, and existing infrastructure. Do not propose rewriting a working app solely to standardize framework names.

Watch for double submissions, duplicate analytics, stale CSRF tokens, duplicate roots, and listeners surviving navigation. Reproduce bugs across hard load, Turbo visit, browser Back, modal/frame navigation, and logout. A feature that works only on the first page load is not integrated correctly.

## Incremental marketplace migration

1. Inventory route URLs, query behavior, public metadata, sessions, forms, API contracts, and analytics.
2. Define ownership and source of truth for data, including IDs and tenant scope.
3. Migrate one route or interaction slice behind an explicit routing/rollout decision.
4. Preserve redirects, canonical URLs, not-found behavior, permission checks, and loading/error states.
5. Compare behavior with fixtures and logs; test both old and new entry points.
6. Roll out gradually with a tested rollback path, then remove obsolete code after confidence and ownership are clear.

For synchronization, define retry/idempotency, version ordering, partial failure, and reconciliation. Avoid uncontrolled dual writes. The UI should not present migrated data as current until the contract says it is current.

## Resume drill and answer cues

Draw one page you worked on and mark your exact change. Explain one Rails/React boundary bug, or state honestly if your role did not include lifecycle setup. For the migration, distinguish APIs you built, pages you moved, and permission checks you contributed to from decisions made by others.

**Why did the handler run twice?** Inspect repeated lifecycle setup and missing cleanup. **Why not rewrite all routes at once?** Explain compatibility, rollback, and learning from one production slice. **Can Turbo update a React-owned node?** Avoid competing ownership; design an explicit boundary or integration contract.

Exit gate: explain a complete hybrid-page lifecycle and propose a measurable, reversible migration for one route.
