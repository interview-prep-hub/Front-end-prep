# 27. Next.js production and Pages Router

Previous: [26. Data and caching](26-nextjs-data-mutations-and-caching.md) | Next: [28. TanStack Router](28-tanstack-router.md)

## Production questions

Know the target runtime, build output, environment variables, image handling, static assets, server dependencies, and deployment cache behavior. A fully static export cannot provide every request-time server feature. Serverless functions, a long-running Node server, and edge runtimes have different APIs, execution models, connection behavior, and limits. Choose from actual requirements and platform support.

Client-exposed `NEXT_PUBLIC_` values are public and generally inlined into client code at build time. A private server variable must stay out of client imports, serialized props, and logs. Preview and production environments can differ in origins, cookies, redirects, and payment/webhook configuration.

Use image and font tooling deliberately: correct dimensions/sizes, allowed remote patterns, useful alt text, and appropriate critical-resource loading. Verify actual generated output rather than assuming a component name guarantees good performance. Inspect source maps, bundle sizes, hydration, and error reporting in production-like builds.

## Proxy and authorization

Current Next.js uses the `proxy` convention for functionality formerly named Middleware, with version/runtime differences worth identifying in older apps. See the [Proxy reference](https://nextjs.org/docs/app/api-reference/file-conventions/proxy). Use it for suitable request routing, redirects, or coarse checks. Keep authoritative authorization at the data/action/endpoint boundary; an entry-layer check does not cover every invocation automatically.

Review cache headers, security headers, external image/redirect inputs, and observability. Avoid expensive database work in broad request interception unless the deployment and requirement justify it. Test direct requests to protected operations.

## Pages Router knowledge for existing applications

| Pages Router concept | Purpose | App Router comparison |
| --- | --- | --- |
| `pages/index.tsx`, dynamic file routes | Route UI | `app/**/page.tsx` with layouts |
| `_app` | Shared application wrapper | Root/nested layouts and providers |
| `_document` | Custom document structure | Root layout document shell |
| getServerSideProps | Request-time server data for a page | Async server rendering/data services, with explicit model |
| getStaticProps | Build/static generation data | Static/cached rendering under App Router rules |
| getStaticPaths | Paths for eligible dynamic static pages | generateStaticParams and related configuration |
| `pages/api/*` | HTTP API routes | Route Handlers |
| `next/router` | Pages Router navigation hooks | App Router `next/navigation` APIs |

These are conceptual correspondences, not search-and-replace migrations. Do not put `getServerSideProps` in `app/`. Data returned as page props is sent to the client, so server execution alone does not make that data secret. The [getServerSideProps guide](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-server-side-props) explains its scope.

Understand older client data fetching and Incremental Static Regeneration at a practical level. Ask which router/version the interviewer means before answering a defaults question. Avoid adopting `getInitialProps` for a new feature without understanding its legacy behavior and impact.

## Migration and release exercise

Plan migration of one housing detail URL from Pages Router or Rails to App Router. Preserve canonical URL, status/not-found behavior, metadata, auth/session expectations, analytics, and redirects. Decide how both applications coexist. Add a rollout flag and a rollback path before expanding to more routes.

Build and run the production app using its generated scripts. Do not assume `next build` runs every lint/test check; run the configured checks explicitly. Test cold load, client navigation, refresh, logged-out access, an expired session, an invalid resource, and stale cached content.

## Questions and answer cues

**Can every Next app be hosted as static files?** Only if its features fit export constraints. **Why does an environment change require rebuilding?** Client-inlined configuration is baked into the build. **How would you migrate safely?** One route/feature slice, contract parity, observability, gradual rollout, and tested rollback.

Exit gate: explain your runtime/deployment choice and compare App Router with Pages Router without mixing their APIs.
