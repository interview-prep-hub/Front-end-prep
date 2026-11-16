# 42. Official resources and version policy

Previous: [41. Revision](41-final-revision-cheatsheet.md) | Next: [43. Progress tracker](43-progress-tracker.md)

Framework references were checked on 2026-11-05. Links below are primary documentation, standards-oriented references, or maintainer guides. Follow the focused chapter first; these references provide deeper contracts and current setup details. Their contents can change independently of these notes.

## Version-sensitive points

| Area | What to verify before using an example |
| --- | --- |
| React | Installed major/minor, enabled compiler, support for Actions, Effect Events, Activity, and framework integration |
| Next.js | App versus Pages Router, installed major, async request APIs, Cache Components/configuration, revalidation API contexts, Proxy convention |
| Vite | Installed major, Node/template engines, bundler/plugin behavior, type-check scripts, env exposure |
| TanStack Router | File generation, typed registration, search schema integration, loader dependencies, route context |
| TanStack Start | Scaffold/build setup, server-function validation API, import protection, SSR Query integration, hosting adapter, experimental features |
| TanStack Query | Major version, object options, status meanings, staleTime/gcTime, retries, SSR instance lifetime |
| React Router | Installed major and declarative/data/framework mode before copying imports or route APIs |
| oRPC / Zod | Installed major, schema/handler APIs, middleware/context, compatible client/server integration |
| Rails / React on Rails | Actual Rails and integration versions, bundler, Turbo lifecycle hooks, SSR edition/configuration |
| Expo / React Native | SDK and native runtime compatibility, architecture/library support, build/update constraints |

A reliable practice workflow is: scaffold once, record versions and config, keep a lockfile, reproduce examples, then upgrade deliberately. `latest` in a setup command is a discovery convenience, not a reproducible version pin. Never claim a new default applies to an older employer codebase without inspecting it.

## Platform, language, and accessibility

- [MDN CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS): origins, preflight, credentials, and response access.
- [MDN closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures): lexical environments and practical closure behavior.
- [MDN microtasks](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide): promise/microtask scheduling.
- [MDN CSS cascade](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Cascade/Introduction): declaration precedence.
- [TypeScript narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html): guards, control flow, and discriminants.
- [W3C modal dialog pattern](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/): keyboard and focus interaction contract.
- [Web Vitals](https://web.dev/articles/vitals): current performance metrics and field assessment.

## React and application libraries

- [React Learn](https://react.dev/learn): component and state foundations.
- [useEffect](https://react.dev/reference/react/useEffect), [useEffectEvent](https://react.dev/reference/react/useEffectEvent): external synchronization and current Effect Event rules.
- [useActionState](https://react.dev/reference/react/useActionState), [useOptimistic](https://react.dev/reference/react/useOptimistic), [use](https://react.dev/reference/react/use): action/resource APIs.
- [Server Components](https://react.dev/reference/rsc/server-components), [Activity](https://react.dev/reference/react/Activity), [React Compiler](https://react.dev/learn/react-compiler): advanced rendering and tooling boundaries.
- [Redux Essentials](https://redux.js.org/tutorials/essentials/part-1-overview-concepts), [Zustand introduction](https://zustand.docs.pmnd.rs/learn/getting-started/introduction): shared client-state approaches.
- [React Router modes](https://reactrouter.com/start/modes): choose the correct routing model.
- [Query defaults](https://tanstack.com/query/latest/docs/framework/react/guides/important-defaults), [optimistic updates](https://tanstack.com/query/latest/docs/framework/react/guides/optimistic-updates), [advanced SSR](https://tanstack.com/query/latest/docs/framework/react/guides/advanced-ssr): remote state lifecycle.
- [React Hook Form](https://react-hook-form.com/get-started), [Zod](https://zod.dev/basics): forms and runtime schemas.

## Frameworks

- [Vite setup](https://vite.dev/guide/), [features](https://vite.dev/guide/features), [environment/modes](https://vite.dev/guide/env-and-mode).
- [Next.js App Router](https://nextjs.org/docs/app), [server/client components](https://nextjs.org/docs/app/getting-started/server-and-client-components), [data mutations](https://nextjs.org/docs/app/getting-started/mutating-data).
- [Next.js caching](https://nextjs.org/docs/app/getting-started/caching), [use cache](https://nextjs.org/docs/app/api-reference/directives/use-cache), [previous caching model](https://nextjs.org/docs/app/guides/caching-without-cache-components), [revalidation](https://nextjs.org/docs/app/getting-started/revalidating).
- [Next.js Proxy](https://nextjs.org/docs/app/api-reference/file-conventions/proxy), [Pages Router getServerSideProps](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-server-side-props).
- [TanStack Router search](https://tanstack.com/router/latest/docs/guide/search-params), [loading](https://tanstack.com/router/latest/docs/guide/data-loading).
- [Start overview](https://tanstack.com/start/latest/docs/framework/react/overview), [setup](https://tanstack.com/start/latest/docs/framework/react/getting-started), [build configuration](https://tanstack.com/start/latest/docs/framework/react/build-from-scratch).
- [Start server functions](https://tanstack.com/start/latest/docs/framework/react/guide/server-functions), [Query integration](https://tanstack.com/start/latest/docs/framework/react/guide/tanstack-query), [authentication](https://tanstack.com/start/latest/docs/framework/react/guide/authentication), [middleware](https://tanstack.com/start/latest/docs/framework/react/guide/middleware).
- [oRPC getting started](https://orpc.dev/docs/getting-started): typed operation contracts and current major conventions.

## Testing, security, and delivery

- [Testing Library queries](https://testing-library.com/docs/queries/about/), [Vitest](https://vitest.dev/guide/), [Playwright practices](https://playwright.dev/docs/best-practices).
- [OWASP authorization](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html), [XSS prevention](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html), [CSRF prevention](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html).
- [GraphQL queries](https://graphql.org/learn/queries/), [MDN SSE](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events).
- [GitHub Actions syntax](https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax).

## Resume-specific extensions

- [React on Rails](https://reactonrails.com/docs/), [Rails JavaScript](https://guides.rubyonrails.org/working_with_javascript_in_rails.html), [Turbo Drive](https://turbo.hotwired.dev/handbook/drive), [Stimulus lifecycle](https://stimulus.hotwired.dev/reference/lifecycle-callbacks).
- [React Native performance](https://reactnative.dev/docs/performance), [FlatList](https://reactnative.dev/docs/flatlist), [architecture](https://reactnative.dev/architecture/landing-page).
- [Expo store builds](https://docs.expo.dev/deploy/build-project/), [runtime versions](https://docs.expo.dev/eas-update/runtime-versions/), [SecureStore](https://docs.expo.dev/versions/latest/sdk/securestore/).

Exit gate: record the versions in your actual practice app and identify which documentation pages govern its cache, runtime, and authorization boundaries.
