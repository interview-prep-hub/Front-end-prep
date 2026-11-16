# Front-end interview preparation

A complete interview study path for JavaScript, TypeScript, React, React with Vite, Next.js, and TanStack Start, with additional preparation for React on Rails, Hotwire, and React Native/Expo.

Read numbered files in ascending order. The order follows prerequisites: the web platform, JavaScript, React, production engineering, frameworks, then interview rehearsal. It is a learning sequence, not a history of framework release dates.

The examples use marketplace, housing, resident, inventory, and learning-platform scenarios connected to your resume. Practice designs are suggestions; they do not assert that you implemented those designs at work. No personal contact information is copied here.

## Start here

1. Read [00: Study roadmap](00-study-roadmap.md) and choose a schedule.
2. Complete the diagnostic in [01: Resume and interview scope](01-resume-and-interview-scope.md).
3. Study each guide, implement its exercise, and answer its questions aloud before checking the answer cues.
4. Record evidence and spaced reviews in [43: Progress tracker](43-progress-tracker.md).
5. Build one project from [37: Capstones](37-capstone-projects.md), then run [40: Mock interviews](40-mock-interviews-and-readiness.md).

## Ordered curriculum

| Stage | Guides, in order | Result |
| --- | --- | --- |
| Orientation | [00 Roadmap](00-study-roadmap.md), [01 Resume map](01-resume-and-interview-scope.md) | A realistic plan and evidence inventory |
| Web platform | [02 Browser and HTTP](02-web-browser-and-http.md), [03 HTML, accessibility, SEO](03-html-semantics-accessibility-and-seo.md), [04 CSS foundations](04-css-foundations-and-layout.md), [05 Responsive styling](05-responsive-css-and-ui-styling.md) | Build and explain usable pages |
| Language | [06 JavaScript foundations](06-javascript-foundations.md), [07 Functions and memory](07-javascript-functions-objects-and-memory.md), [08 Async](08-javascript-async-and-event-loop.md), [09 Browser APIs](09-dom-and-browser-apis.md), [10 Coding patterns](10-data-structures-and-javascript-coding.md), [11 TypeScript](11-typescript-for-front-end.md) | Solve language and browser problems |
| React applications | [12 Vite and tooling](12-tooling-and-react-with-vite.md), [13 Rendering](13-react-rendering-and-components.md), [14 Hooks](14-react-hooks-and-effects.md), [15 Advanced React](15-advanced-react-and-modern-apis.md), [16 State](16-state-management-and-architecture.md), [17 Routing](17-client-routing-and-url-state.md), [18 Query](18-tanstack-query-and-server-state.md), [19 Forms](19-forms-validation-and-file-uploads.md), [20 APIs and real-time](20-api-contracts-and-real-time-ui.md) | Implement reliable interactive features |
| Production foundations | [21 Testing](21-front-end-testing.md), [22 Performance](22-performance-and-debugging.md), [23 Security and tenants](23-security-authentication-and-multi-tenancy.md), [24 Rendering strategies](24-rendering-strategies-and-hydration.md) | Explain correctness, speed, and boundaries |
| Next.js | [25 App Router](25-nextjs-app-router.md), [26 Data and caching](26-nextjs-data-mutations-and-caching.md), [27 Production and Pages Router](27-nextjs-production-and-pages-router.md) | Defend an App Router implementation and discuss older apps |
| TanStack | [28 Router](28-tanstack-router.md), [29 Start](29-tanstack-start.md), [30 Data, auth, oRPC](30-tanstack-start-data-auth-and-orpc.md) | Explain typed routing, SSR, and server boundaries |
| Resume extensions | [31 Rails and migration](31-react-on-rails-hotwire-and-migration.md), [32 Native and Expo](32-react-native-and-expo.md) | Handle questions prompted by your listed experience |
| Design and delivery | [33 System design](33-front-end-system-design.md), [34 Component design](34-component-and-design-system-interviews.md), [35 Delivery and observability](35-git-ci-deployment-and-observability.md) | Reason about growth, maintenance, and release risk |
| Rehearsal | [36 Machine coding](36-machine-coding-rounds.md), [37 Capstones](37-capstone-projects.md), [38 Resume and behavioral](38-resume-deep-dives-and-behavioral.md), [39 Question bank](39-interview-question-bank.md), [40 Mocks](40-mock-interviews-and-readiness.md) | Deliver under interview conditions |
| Review tools | [41 Cheatsheet](41-final-revision-cheatsheet.md), [42 Official resources](42-official-resources-and-version-policy.md), [43 Tracker](43-progress-tracker.md) | Retain knowledge and check changing APIs |

## Working conventions

- Core means required for ordinary React interviews. Stretch means deeper follow-ups for experienced roles. The resume-specific chapters matter whenever that technology remains on your resume.
- Code blocks are focused examples unless a guide explicitly labels a complete file. Named application services such as `requireSession` are application code, not framework exports.
- Commands use PowerShell-compatible syntax. Scaffold practice apps in new subdirectories, keeping these notes intact. You do not need to install anything just to read the material.
- This is broad interview coverage, not an exhaustive language specification. Use the official references for full API contracts and role-specific additions.
- Framework references were checked on 2026-11-05. Check the installed version and configuration before applying an example to an existing app.

## Your definition of done

You can explain the behavior, implement it without copying, test an unhappy path, and discuss one tradeoff. A checked reading box alone is not completion.
