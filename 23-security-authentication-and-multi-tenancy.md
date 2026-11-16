# 23. Security, authentication, and multi-tenancy

Previous: [22. Performance](22-performance-and-debugging.md) | Next: [24. Rendering](24-rendering-strategies-and-hydration.md)

## Boundaries to understand

Authentication establishes who is making a request. Authorization decides whether that identity may perform this operation on this resource. Tenant isolation restricts which organization's data participates. Input validation checks shape and constraints; it does not establish permission.

A hidden button, protected client route, TypeScript type, or unguessable-looking ID is not an authorization boundary. Enforce permissions for every relevant server request and deny by default. See [OWASP's authorization guidance](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html).

Illustrative server algorithm, not a framework API:

```text
parse input
read authenticated session from a trusted request boundary
verify current membership in the requested organization
query the resource using both organization ID and resource ID
check action permission and relevant business constraints
perform the operation, using transaction/version/idempotency rules as needed
return the minimum authorized response
```

Do not trust an `orgId`, `role`, `userId`, or ownership flag just because the client supplied it. Derive identity from the trusted session and verify membership. A record with the same ID in two tenant scopes must not share a client cache entry accidentally.

## Browser threats

XSS executes attacker-controlled script in a trusted page context. React escapes ordinary text, but interpreted HTML, unsafe URLs, third-party scripts, and DOM insertion remain boundaries. Encode for the destination context; sanitize allowed rich HTML with a maintained sanitizer; avoid dangerous HTML insertion where possible. CSP and Trusted Types can provide defense in depth. See [OWASP XSS prevention](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html).

CSRF abuses automatically attached credentials to trigger an unwanted state-changing request. Use the framework's CSRF mechanism, appropriate token/origin verification, safe method semantics, and cookie settings. SameSite is useful defense in depth, but must match actual cross-site flows and should not be treated as the only universal protection. CORS is a separate browser response-access policy. See [OWASP CSRF prevention](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html).

Session cookies commonly use HttpOnly, Secure, and a suitable SameSite policy. HttpOnly reduces direct script access to a cookie; it does not make XSS harmless because script can still act within the user's session. Script-readable tokens, including those in localStorage, are exposed to XSS. Choose a deliberate session architecture rather than a universal token-storage slogan.

OAuth/OIDC browser flows commonly use an authorization-code flow with PKCE and appropriate redirect/state handling. Know the roles at a conceptual level and use maintained provider/framework integrations rather than implementing a protocol from memory.

## Tenant switching and cached private data

Scope query keys, persisted drafts, sockets, subscriptions, and server caches deliberately. On logout, clear private in-memory/persisted state as required. On tenant switch, cancel old requests and ensure late responses cannot populate the new screen. A cache keyed by orgId helps presentation isolation, but server membership checks remain authoritative.

SSR serialized data is delivered to the browser. Do not dehydrate secret fields merely because the code that fetched them ran on the server. Public/CDN caching of personalized output can create a cross-user leak. Verify cache policy independently from authentication logic.

## Other front-end concerns

Validate redirect destinations to avoid open redirects. Keep private keys out of client environment variables and bundles. Handle uploaded content and external links deliberately. Do not log session tokens or sensitive form data. A front-end subscription flag can control UI, but the server must check current entitlement for protected data/actions.

## Practice and questions

Create a two-organization test matrix: unauthenticated, valid member, different-org member, read-only member, expired membership, and guessed record ID. Test direct calls, not just UI navigation. Repeat after switching organizations with a delayed request in flight.

**Does JWT mean stateless and secure by default?** No; validation, expiry, revocation/rotation needs, storage, and permissions still need design. **Can GET mutate a task safely?** State changes should use appropriate mutation semantics and protections. **Where should the tenant filter live?** At trusted server data-access boundaries, with client scoping for correct UX.

Exit gate: explain XSS versus CSRF and show server-enforced resource/tenant checks plus a cache-isolation test.
