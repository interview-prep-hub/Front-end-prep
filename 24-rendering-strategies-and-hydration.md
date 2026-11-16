# 24. Rendering strategies and hydration

Previous: [23. Security](23-security-authentication-and-multi-tenancy.md) | Next: [25. Next.js App Router](25-nextjs-app-router.md)

## Separate the decisions

| Strategy/concept | Main idea | Typical tradeoff |
| --- | --- | --- |
| CSR | Browser JavaScript produces the interactive UI | Simple static hosting, but startup and initial content can depend on JS/data |
| SSR | Server produces HTML for a request | Earlier HTML, server cost, cache and hydration design |
| Static generation | HTML produced before a request | Fast reusable delivery, content freshness/rebuild strategy |
| Revalidation/ISR | Refresh previously generated/cached output under a framework policy | Availability and staleness tradeoffs |
| Streaming | Deliver useful parts before all work finishes | Boundary design, fallbacks, error/status timing |
| Hydration | Attach React behavior to compatible server-produced markup | Client code and initial-state agreement required |
| Server Components | Components run in a server/build environment and contribute a serialized result | Server/client composition and bundler/framework constraints |

SSR and Server Components are different axes. A Client Component can be included in server-generated initial HTML. A Server Component can run at build time or on a server; it does not mean every user request always re-executes the component. See [React Server Components](https://react.dev/reference/rsc/server-components).

## A rendering decision example

For a housing marketplace, public listing/detail content benefits from crawlable initial HTML and an explicit freshness policy. Interactive filters need client behavior. A private resident dashboard can use SSR or CSR depending on latency, auth, operations, and user needs. There is no requirement to render every page identically.

Ask: who needs the initial content, how personalized is it, how often does it change, what is acceptable staleness, what client code is necessary, and where can it be hosted? A complex rendering strategy is not automatically better than a well-built SPA.

## Hydration failures

The server and client's initial output must agree. Common causes include reading localStorage during initial client render, time/randomness, locale/time-zone differences, invalid HTML nesting repaired by the browser, browser extensions, and conditional browser-global checks that change markup.

Use deterministic initial data, valid markup, an explicit locale/time-zone strategy, and appropriate client lifecycle handling for browser-only information. `suppressHydrationWarning` is a targeted escape hatch, not a general fix. Rendering nothing until mount can remove a useful initial experience, so choose it only for a justified client-only region.

Request-specific state must not live in a server module singleton. Create a store/cache with the correct request scope, serialize only permitted state, then initialize a stable browser instance. Keep serialized values compatible with the framework transport.

## Streaming and waterfalls

A slow parent awaiting data can delay independent child work. Start independent work together when safe, and place Suspense boundaries where partial content is meaningful. Avoid exposing personalized content through a shared static shell or cache. Once a streamed response has committed headers, status/error handling may differ from an unstarted response, so plan route errors and logging.

SEO also needs titles, canonicalization, links, metadata, useful status behavior, and good content. A streamed shell with no useful content for the intended consumer may not meet the requirement just because it is called SSR.

## Practice and questions

Render a timestamp with different server/client time zones and explain the mismatch. Fix it with a deliberate initial representation and subsequent enhancement if required. Compare a public detail page, private tasks page, and static help page in a decision table with freshness, runtime, and cache policy.

**Does 'use client' mean never rendered on the server?** No; it defines a client module boundary in an RSC-enabled framework. **Does SSR remove JavaScript cost?** Not necessarily; hydration and client components still cost work. **Is streaming a database optimization?** No; it changes delivery and scheduling, while slow queries may remain slow.

Exit gate: distinguish SSR, RSC, static generation, streaming, and hydration with one concrete page design.
