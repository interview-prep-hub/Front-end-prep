# 02. Web, browser, and HTTP

Previous: [01. Resume map](01-resume-and-interview-scope.md) | Next: [03. HTML and accessibility](03-html-semantics-accessibility-and-seo.md)

## The request-to-pixel model

A navigation may involve URL parsing, cached DNS lookup, connection setup, TLS, an HTTP request, server work, response transfer, HTML parsing, subresource discovery, style calculation, layout, paint, and compositing. Caches and connection reuse can skip work. HTTP/3 uses QUIC rather than the TCP transport used by HTTP/1.1 and HTTP/2.

The DOM represents document structure. CSS rules contribute to computed styles. Layout calculates geometry; paint produces drawing commands; compositing assembles layers. JavaScript can change the DOM and styles, which can trigger more work. Reading geometry after writing styles can force synchronous layout.

Blocking scripts can pause parsing. `defer` scripts execute after parsing, preserving their document order. `async` scripts execute when ready without that order guarantee. Module scripts are deferred by default, with their dependency graphs also affecting loading. Use the Network and Performance panels to verify a particular page.

## HTTP concepts to explain

| Concept | Meaning and example |
| --- | --- |
| Safe method | Intended to read rather than mutate, such as GET |
| Idempotence | Repeating the same intended operation has the same effect; PUT is defined this way, POST is not generally |
| 200 / 201 / 204 | Success with a representation / creation / success without a response body |
| 301 / 302 / 307 / 308 | Redirects with different permanence and method-preservation semantics |
| 304 | Reuse a cached representation after conditional validation |
| 400 / 401 / 403 / 404 | Invalid request / authentication required / forbidden / absent or deliberately undisclosed |
| 409 / 422 / 429 | Conflict / semantically invalid input / rate limit |
| 5xx | A server or upstream failure, not a reason to retry every operation blindly |

`Content-Type` describes the body; `Accept` describes supported response formats. `ETag` plus `If-None-Match` enables validation. `Cache-Control: no-cache` allows storage but requires validation before reuse; `no-store` prohibits storing the response under that cache directive. `private` prevents shared-cache storage but does not mean encryption.

Explain browser cache, CDN cache, framework data cache, and client query cache separately. Inventory and market prices may have different acceptable staleness. A CDN must never mix personalized responses across users or tenants.

## Origins, cookies, and CORS

An origin combines scheme, host, and port. Same-site and same-origin are different concepts. Cookies are sent according to domain/path, security, SameSite, and credential rules. An HttpOnly cookie is unavailable to browser JavaScript but can still be attached to requests.

CORS controls a browser script's access to cross-origin responses. Some requests require a preflight. A credentialed response needs an explicit allowed origin, not `*`; the server also needs appropriate credential headers. CORS is not authentication, and `mode: 'no-cors'` produces an opaque response rather than fixing an API integration. See [MDN's CORS guide](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS).

## Debugging exercise

Inspect a listing page with DevTools. Record document TTFB, image transfer size, a blocking script, cache response headers, and one layout event. Disable cache and throttle the connection, then compare a repeat navigation. Distinguish server time from download time and JavaScript execution.

Reproduce a failing API call. Ask: did DNS/connect fail, did preflight fail, did the response have an HTTP error, did body parsing fail, or did UI state handling fail? `fetch` usually resolves for HTTP 4xx/5xx, so check `response.ok`.

## Questions and answer cues

**What happens when you type a URL?** Describe the layers, reuse/caching opportunities, and rendering, rather than treating the sequence as one mandatory network path.

**Why does a request work in an API client but fail in a browser?** Browser origin/credential enforcement may differ; inspect preflight and response headers before changing server policy.

**Why can a page download quickly but feel slow?** Main-thread JavaScript, long tasks, layout, images, and hydration may dominate after transfer.

Exit gate: diagnose a request using evidence from headers and timing, and explain one cache policy for public listings versus private resident data.
