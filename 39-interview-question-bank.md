# 39. Interview question bank

Previous: [38. Resume and behavioral](38-resume-deep-dives-and-behavioral.md) | Next: [40. Mocks and readiness](40-mock-interviews-and-readiness.md)

Answer the question before reading its cue. Give a definition, a concrete example, and one limitation. The cues are concise model answers, not scripts to memorize. Select ten mixed questions per day and expand weak answers using the linked guides. For framework behavior, state the version/configuration assumed.

## Web, HTML, and CSS, 1-10

Review [02](02-web-browser-and-http.md), [03](03-html-semantics-accessibility-and-seo.md), [04](04-css-foundations-and-layout.md), and [05](05-responsive-css-and-ui-styling.md).

1. **What happens after entering a URL?** Resolve/reuse network information and connections, request content, parse resources, calculate style/layout, paint, then run/attach interaction. Caches can bypass steps.
2. **no-cache versus no-store?** no-cache permits storage but requires validation before reuse; no-store prohibits storage under that directive. Neither means encrypted.
3. **Does fetch reject for a 404?** Normally no. Inspect status/ok, then handle HTTP failure separately from network or JSON parsing failure.
4. **Button versus anchor?** A button performs an action; an anchor navigates. Native semantics carry keyboard and browser behavior.
5. **What makes a field accessible?** A useful label/name, correct type/role, instructions/errors association, keyboard behavior, and discoverable validation/progress.
6. **Is ARIA enough for a modal?** No. Naming, initial focus, keyboard containment, Escape, background interaction, and focus restoration must work.
7. **How is a CSS winner chosen?** Evaluate the cascade's origin/importance/layers before specificity and later tie breakers. The most specific selector is not an unconditional winner.
8. **Flexbox or Grid?** Choose by layout relationships: flex is useful for main-axis distribution, grid for shared two-dimensional tracks. Requirements can overlap.
9. **Why does a flex child overflow?** Its intrinsic minimum may prevent shrinking. Inspect min-size, long content, fixed dimensions, and containing layout before hiding overflow.
10. **Why can huge z-index fail?** The element may be trapped in a lower stacking context or clipped by an ancestor. Fix the context/placement.

## JavaScript values, functions, and objects, 11-20

Review [06](06-javascript-foundations.md) and [07](07-javascript-functions-objects-and-memory.md).

11. **Does const make an object immutable?** No. It prevents binding reassignment; object properties can still change unless separately constrained.
12. **What is the temporal dead zone?** A lexical binding exists but is inaccessible before initialization. Access during that interval throws rather than yielding undefined.
13. **What is a closure?** A function retains access to its lexical environment. It can observe changing bindings; each factory invocation can create independent state.
14. **What determines this?** Regular-function invocation determines it; arrows capture lexical this. Detaching a method can lose its receiver.
15. **call, apply, bind?** call/apply invoke with explicit this using positional/array-like arguments; bind returns a function with a receiver and optional arguments bound.
16. **Why does object spread not isolate nested state?** It copies one level. Nested objects/arrays remain shared references unless updated separately.
17. **=== versus Object.is?** Both use identity for objects; Object.is treats NaN as equal to itself and distinguishes positive/negative zero.
18. **Why prefer ?? over || for a default quantity?** Zero is a valid quantity in many domains. ?? only replaces null/undefined, while || replaces all falsy values.
19. **How does prototype lookup work?** Missing own properties are searched through the prototype chain. Use an own-property check when inherited properties should not count.
20. **How can a closure contribute to a leak?** A long-lived callback can retain its environment and referenced objects. Correctly releasing its owner/listener usually solves the lifetime problem.

## Async and browser behavior, 21-30

Review [08](08-javascript-async-and-event-loop.md) and [09](09-dom-and-browser-apis.md).

21. **Promise reaction or zero-delay timer first?** After current synchronous work, queued promise microtasks normally run before the timer task in the ordinary browser example.
22. **Does await block the browser?** It suspends that function's continuation. Synchronous work before/after an await still runs and can block the main thread.
23. **Does Promise.all cancel siblings on failure?** No. It rejects its aggregate; running operations need their own cancellation mechanism.
24. **allSettled, any, race?** allSettled reports every outcome; any fulfills on the first fulfillment; race settles with the first settlement.
25. **Why is forEach(async ...) misleading?** forEach does not await callback promises. Use sequential iteration or an explicit promise aggregation/concurrency policy.
26. **Debounce versus throttle?** Debounce waits for inactivity under its contract; throttle limits frequency during continued activity. Define leading/trailing behavior.
27. **How do you prevent stale search results?** Key results to the active request/query, abort obsolete work where supported, and ignore responses no longer belonging to the current operation.
28. **target versus currentTarget?** target identifies the originating element; currentTarget identifies the element whose listener is currently handling the event.
29. **Worker versus service worker?** A worker supports off-main-thread computation; a service worker has an event-driven network/cache-related lifecycle for eligible pages.
30. **localStorage versus IndexedDB?** localStorage is synchronous string storage for small values; IndexedDB is async structured storage with schema/transaction concerns. Neither is automatic authorization.

## TypeScript and coding patterns, 31-40

Review [10](10-data-structures-and-javascript-coding.md) and [11](11-typescript-for-front-end.md).

31. **unknown versus any?** unknown requires narrowing before use; any permits unchecked operations. Prefer unknown plus validation for untrusted values.
32. **Do TypeScript types validate JSON?** No. Parse/check runtime data, then use the resulting trusted type.
33. **Union versus intersection?** A union accepts one of multiple alternatives; an intersection requires satisfying the combined constraints.
34. **What is a discriminated union?** Alternatives share a literal tag that narrows the associated fields, supporting valid-state modeling and exhaustive handling.
35. **What does satisfies do?** It checks compatibility while retaining useful expression inference. It does not convert values or freeze runtime objects.
36. **What makes a useful generic?** It preserves a meaningful relationship among input/output types rather than replacing explicit domain types with unnecessary abstraction.
37. **Map or object?** Use Map for arbitrary keyed collections and collection operations; objects suit many named records. Consider key types, iteration, and prototype behavior.
38. **How do you remove duplicate records?** Define winner and ordering policy, then use keyed tracking. A Map can retain first insertion order while replacing stored values.
39. **BFS versus DFS?** BFS explores by depth using a queue; DFS follows paths using a stack/recursion. Discuss memory, graph cycles, and recursion depth.
40. **How do you analyze a sliding window?** State its invariant and how far each pointer advances. Two nested-looking movements can still total linear work.

## React rendering and component identity, 41-50

Review [13](13-react-rendering-and-components.md).

41. **Render versus commit?** Rendering calculates the UI; committing applies host changes. Rendering may restart or produce no DOM change.
42. **Why keep render pure?** Repeated or abandoned work must not trigger externally visible side effects or mutate previous snapshots.
43. **Why is state unchanged immediately after setState?** The handler sees its render's snapshot; the setter queues an update rather than rewriting current local variables.
44. **When use a functional update?** When the next state depends on pending previous state, especially when multiple updates can queue together.
45. **What makes a good key?** A stable logical identifier unique among siblings. Keys preserve matching identity rather than being passed as normal props.
46. **Why are array-index keys risky?** Insert/reorder/delete can associate retained state/focus with another logical item. Fixed noninteractive lists may be a different case.
47. **How do you intentionally reset state?** Change the relevant component identity/key when that matches the requirement, or perform an explicit owned state transition.
48. **Controlled versus uncontrolled input?** React state supplies a controlled value; the DOM owns an uncontrolled value. Avoid accidental mode switches.
49. **Why avoid derived state copies?** Two sources of truth can disagree. Calculate from canonical inputs unless a separate editable snapshot is intentional.
50. **Does parent render mean every DOM child changes?** No. Rendering, reconciliation, memo boundaries, and committing are distinct steps.

## Hooks, scheduling, and modern React, 51-60

Review [14](14-react-hooks-and-effects.md) and [15](15-advanced-react-and-modern-apis.md).

51. **What is an effect for?** Synchronizing with an external system. Calculations and event-specific actions often belong in render or an event/action instead.
52. **When does cleanup run?** Before the next changed-dependency synchronization and on unmount, with extra development checks under applicable Strict Mode setup.
53. **Why include dependencies?** They describe reactive values used by the synchronization. Omitting one can retain an outdated closure and incorrect external state.
54. **Ref versus state?** Ref changes do not schedule rendering; state changes do. Use refs for suitable mutable integration data or host references.
55. **Memo versus callback memoization?** memo targets component props comparison; useMemo caches a calculation; useCallback caches function identity. All need a useful boundary.
56. **Does Suspense recognize any fetch?** No. It coordinates supported suspending resources/framework integration, not arbitrary effect-started requests.
57. **What do error boundaries miss?** Ordinary event-handler and unrelated async callback errors generally need local handling outside render error boundaries.
58. **Transition versus debounce?** A transition prioritizes rendering updates; debounce delays starting work according to a time contract. They solve different problems.
59. **What does useActionState provide?** State and pending behavior around an action result. It does not create validation or authorization automatically.
60. **What changes with React Compiler?** Eligible memoization can be automated when configured; pure rendering, sensible state ownership, and measuring actual bottlenecks remain necessary.

## State, routing, and Query, 61-70

Review [16](16-state-management-and-architecture.md), [17](17-client-routing-and-url-state.md), and [18](18-tanstack-query-and-server-state.md).

61. **Where should state live?** With the owner matching its scope/lifetime: local UI, form, URL, shared workflow, or remote cache. Derive values when possible.
62. **Is Context a complete state architecture?** It distributes values; update ownership, reducer logic, subscription granularity, persistence, and server-state management still need decisions.
63. **When choose Redux Toolkit or another store?** When shared client workflow, tooling, conventions, or subscription needs justify it. Team context matters more than a universal size threshold.
64. **What belongs in the URL?** Safe addressable navigation state such as filters/page/sort, when share/refresh/back behavior matters. Avoid secrets and personal drafts.
65. **push versus replace?** Push adds a history step; replace updates the current entry. Choose according to user navigation expectations.
66. **What belongs in a query key?** Every input that changes the identified response, including organization/user/resource/filter/page scope where applicable.
67. **staleTime versus gcTime?** Freshness versus retention of inactive cached data. Stale is not the same as removed.
68. **Why invalidate after a mutation?** Related read caches may be outdated. Update canonical data or refetch the correctly scoped keys.
69. **Why can optimistic rollback be wrong?** An old snapshot may overwrite a newer concurrent update. Use per-entity ordering, operation identity, or robust reconciliation.
70. **Why request-scoped QueryClient in SSR?** A shared private cache can reuse data across requests/users. Transfer only permitted fields to the browser.

## Forms, APIs, and tests, 71-80

Review [19](19-forms-validation-and-file-uploads.md), [20](20-api-contracts-and-real-time-ui.md), and [21](21-front-end-testing.md).

71. **Why validate on both client and server?** Client validation improves feedback; the server must defend the trusted operation against arbitrary requests and business-rule violations.
72. **How should a failed form behave?** Preserve values, show actionable field/global errors, announce status, and support correction/resubmission without ambiguous pending state.
73. **How do you prevent duplicate payments?** Use an authoritative server/provider idempotency design and canonical result. Disabling a button only reduces accidental repeats.
74. **Why let the browser set multipart headers?** It supplies the boundary matching FormData. A manually incomplete Content-Type can break parsing.
75. **Offset versus cursor pagination?** Offset is simple for page numbers but shifts with changing data; cursor approaches need deterministic ordering and a clear continuation contract.
76. **Can GraphQL return data and errors?** Yes. Handle partial results according to the operation/client policy, separately from HTTP/network failure.
77. **Polling, SSE, or WebSocket?** Choose by update frequency and direction, infrastructure, auth, and reconnect needs. Simpler polling can satisfy many progress screens.
78. **Unit versus integration versus end-to-end?** Choose the cheapest layer that can catch the real risk: pure logic, component interactions/contracts, or actual browser/server behavior.
79. **getBy, queryBy, findBy?** Required synchronous lookup, absence-friendly lookup, and async appearance lookup respectively, with the library's matching/error rules.
80. **Why avoid fixed sleeps in browser tests?** They neither guarantee readiness nor adapt to faster/slower environments. Wait on observable conditions with appropriate assertions.

## Performance, security, and rendering, 81-90

Review [22](22-performance-and-debugging.md), [23](23-security-authentication-and-multi-tenancy.md), and [24](24-rendering-strategies-and-hydration.md).

81. **What do LCP, INP, CLS measure?** Largest content timing, interaction responsiveness, and visual layout instability. Use field and lab evidence for different purposes.
82. **What causes layout thrashing?** Repeated writes followed by geometry reads can force layout work. Batch operations and verify with a trace.
83. **Why not virtualize every list?** It adds focus, accessibility, geometry, print, and navigation complexity. Use it when measured/expected scale justifies it.
84. **Authentication versus authorization?** Identity versus permission to act on a resource. An authenticated user can still be forbidden.
85. **XSS versus CSRF?** XSS runs unwanted script in the trusted context; CSRF abuses attached credentials to induce an unwanted request. Defenses address different boundaries.
86. **Does HttpOnly make XSS safe?** No. It prevents direct script reading of the cookie, but injected script can still act using the user's session.
87. **How do you enforce tenant isolation?** Verify trusted identity/membership and scope resource operations server-side; separately scope client caches, subscriptions, and drafts.
88. **SSR versus Server Components?** SSR generates HTML; Server Components define a server/build execution/composition boundary. Client Components can still appear in initial server HTML.
89. **What causes hydration mismatch?** Different initial output, often from time, locale, randomness, browser-only state, invalid nesting, or modified DOM.
90. **Does streaming solve a slow database?** It can deliver independent UI sooner, but does not eliminate slow data work. Design boundaries and backend performance separately.

## Next.js, 91-100

Review [25](25-nextjs-app-router.md), [26](26-nextjs-data-mutations-and-caching.md), and [27](27-nextjs-production-and-pages-router.md).

91. **What does use client mean?** It establishes a client module boundary in App Router/RSC composition. It does not imply the component never participates in server HTML.
92. **Can a client boundary import a database module?** Keep private services server-only. Client imports can pull dependencies into browser builds or fail the framework boundary checks.
93. **Layout versus template?** Layouts preserve shared structure/state according to route identity; templates establish a fresh instance under their documented navigation behavior.
94. **Are params always synchronous objects?** No. Current App Router APIs use async request patterns; match the installed version and await supported request values appropriately.
95. **Are all Next fetches cached?** No. State the version/configuration and cache model, then explain the explicit behavior used by that app.
96. **What do use cache and cacheLife express?** Reusable function/component work and its lifetime under the configured Cache Components model. Scope and privacy still matter.
97. **updateTag versus revalidateTag?** Current APIs distinguish immediate read-your-own-writes expiry in Server Actions from supported stale-while-revalidate behavior using profiles/call contexts.
98. **Does a Server Action need authorization?** Yes, at each trusted operation, with runtime input validation and resource/tenant permission checks.
99. **Route Handler or direct server service?** Use an HTTP handler for HTTP consumers; server rendering can often call a trusted service directly without an extra internal HTTP hop.
100. **Can getServerSideProps run inside app/?** No. It is a Pages Router API. App Router uses its own rendering/data conventions.

## TanStack Router and Start, 101-110

Review [28](28-tanstack-router.md), [29](29-tanstack-start.md), and [30](30-tanstack-start-data-auth-and-orpc.md).

101. **Router, Query, Start?** Routing/lifecycle, remote-state caching, and full-stack application capabilities respectively. Integration does not erase their separate ownership.
102. **What does validateSearch solve?** It parses/checks external URL-derived state and provides a typed usable result or intentional fallback/error behavior.
103. **What does loaderDeps solve?** It declares relevant search dependencies for a route loader's result identity and reload behavior.
104. **What is beforeLoad for?** Appropriate route context and navigation checks before loading. It does not replace permission checks in directly callable server operations.
105. **Is a route loader server-only?** Not by assumption; it can participate in server rendering and browser navigation. Use a supported server boundary for private code.
106. **What does createServerFn provide?** A supported transformed client/server call boundary with method, validation, and handler conventions. Security policy remains application work.
107. **Does annotating input as a type validate it?** No. Use a runtime validator/schema and then authorize the operation.
108. **How should Start integrate Query for SSR?** Use request-scoped construction and the supported integration, with one deliberate client/provider and safe hydration of authorized data.
109. **Why might oRPC coexist with Start?** Shared typed procedures/contracts or multiple consumers may justify it; avoid redundant layers without a clear responsibility.
110. **Are ordinary Start server functions RSC?** No. SSR, server functions, and Server Components are different concepts; current RSC support has its own experimental boundary.

## Design, delivery, mobile, and resume, 111-120

Review [31](31-react-on-rails-hotwire-and-migration.md) through [38](38-resume-deep-dives-and-behavioral.md).

111. **What goes wrong mixing Turbo and React?** Competing DOM ownership, duplicate mounts/listeners, stale snapshots, and missing teardown can break navigation lifecycle correctness.
112. **How do you migrate a marketplace page?** Preserve route/SEO/auth/contracts, migrate a bounded slice, compare behavior, observe rollout, and maintain a tested rollback path.
113. **What transfers from React web to Native?** State/composition concepts and some domain logic; DOM, styling, navigation, permissions, and host behavior need platform-specific treatment.
114. **Can an Expo update add missing native code?** Not to an incompatible existing binary. Match runtime compatibility and build a suitable binary when native capabilities change.
115. **What should happen on a notification tap?** Parse the destination, handle cold/warm start, restore auth/navigation, recheck access, and retrieve current authorized data.
116. **How do you start a design round?** Clarify users, scope, constraints, data, freshness, and failure priorities before choosing libraries or drawing services.
117. **When use a micro-frontend?** Independent ownership/deployment may justify it, but weigh shared runtime/auth/design/observability costs against a modular single application.
118. **Why can production fail after green CI?** Environment/runtime differences, stale assets, real data/origins, incompatible contracts, and untested deployment flows remain possible.
119. **How do you explain product scale honestly?** Attribute total platform scale as context, then state your specific contribution and evidence without claiming personal ownership of all traffic.
120. **What is a convincing bug story?** A reproducible symptom, hypothesis and evidence, your change, verification, and reflection. Do not invent impact metrics or responsibility.

## Retrieval exercise

Choose one question from each group, allow 90 seconds per answer, and record the answers. Mark incorrect facts, vague tradeoffs, and examples you cannot implement. Revisit the underlying chapter, then answer the same question with a changed scenario rather than memorizing the previous wording.

Exit gate: answer at least eight of ten randomly selected core questions accurately, with a concrete example and no critical misconception about state, server boundaries, or permissions.
