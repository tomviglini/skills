# Production Delivery

Read this when establishing or changing production cache behavior, or investigating a difference between configured and observed delivery. Reuse the resulting project contract for ordinary work.

## Trace the response through the actual stack

Map the affected request from the browser through the CDN and reverse proxy to the origin, omitting layers the project does not use. Static hosting may serve build artifacts without passing through application middleware. An origin header change has no effect on a route that bypasses that origin.

For each layer, identify:

- Which configuration sets response headers for this path and status: application code, static-server settings, hosting manifests, or provider rules.
- Whether it forwards, replaces, removes, or consumes upstream headers, and which matching rule wins.
- Whether it stores the response at all, its effective freshness and stale-serving policy, and the cache key. Check relevant path, query, representation, cookie, and authorization behavior without broadening public caching to private content.

The main production finding is the CDN's actual behavior. Establish these separately for each affected resource class:

| Question | Evidence to establish |
| --- | --- |
| Does the CDN cache it? | Route eligibility, bypass conditions, matching rules, and observed cache status where available. A forwarded `Cache-Control` header alone proves none of these. |
| For how long is it fresh there? | The effective edge TTL and its source: an origin directive, targeted CDN header, provider default, or overriding rule. Include any permitted stale-serving window separately. |
| What reaches the browser? | The final cache policy after forwarding, consumption, or rewriting, plus browser reuse when tested. It may intentionally differ from the CDN policy. |

For the one-year immutable default, verify whether the CDN also uses that lifetime or has a different explicit policy. `Age` reports age, not the configured TTL. A freshness lifetime does not guarantee that a cache retains the object for that entire period; eviction and bypass are separate from expiration. Distinguish configured TTL from observed reuse or expiry, and leave unavailable evidence unverified rather than inferring it from the browser header.

Separate the policy used internally by an intermediary from the headers it forwards. A cache hit can be served without contacting the origin. The browser's received headers alone therefore do not reveal every intermediary's policy. Zero browser freshness also does not guarantee a fresh origin lookup: a validating request can reach an intermediary that already holds a usable response.

Preserve the intended distinction between browser freshness and shared-cache freshness. A CDN purge does not clear browser copies, and an origin header change does not instantly replace already fresh cached responses. If a service worker exists, account for its separate response-selection behavior when relevant.

## Check provider precedence when that provider is present

Consult the deployed product's current official documentation and actual configuration; do not assume every proxy honors the same rules. These examples explain why inspection matters, not which provider to adopt:

- **Vercel:** its targeted `Vercel-CDN-Cache-Control` takes precedence over `CDN-Cache-Control`, which takes precedence over `Cache-Control` for its CDN. The Vercel-specific header is consumed before delivery to the browser. With only `Cache-Control`, `s-maxage` is also removed downstream. Its absence from the browser response does not establish that it was ignored. See [Vercel's header precedence](https://vercel.com/docs/caching/cache-control-headers#behavior).
- **Cloudflare:** Edge Cache TTL rules can override shared-cache behavior, while Browser Cache TTL rules can alter downstream freshness. Inspect the active Origin Cache Control setting and matching rules; the origin's header is not sufficient evidence of either result. See [Cloudflare's cache-control interactions](https://developers.cloudflare.com/cache/concepts/cache-control/#interaction-with-other-cloudflare-features).

For other proxies or CDNs, establish the equivalent behavior without copying these header names or priorities. Verify that negotiated representations use appropriate keys and `Vary` handling. If versioning uses a query parameter, verify both that caches distinguish it and that the origin serves the intended version.

## Verify through the published URL

1. Use representative ordinary GET requests for an immutable asset, the root/HTML, a mutable fixed file, and a missing asset. Inspect status, content type, effective cache headers, validators, and returned content through the actual public hostname. When origin access is already available, compare its response with the public response to locate overrides; do not bypass access controls to obtain it.
2. Repeat the same URL under comparable request conditions. Inspect `Age`, cache-status headers, or provider diagnostics when available. Distinguish a hit, miss, bypass, and revalidation using that provider's documented meanings. Confirm the effective edge TTL from its governing configuration and supported diagnostics; check expiry when practical in an authorized test environment without waiting out a long production TTL or changing live settings merely for the test. A `200` or a `304` alone does not identify the responding layer. [RFC 9211](https://www.rfc-editor.org/rfc/rfc9211.html#section-3) illustrates how `Cache-Status` can report hits and forwarding through multiple caches.
3. Test browser reuse separately with caching enabled. Identify memory/disk reuse, network requests, and service-worker responses where present. Cache-busting parameters, forced reloads, and developer tools that disable caching change the experiment; use them only for the specific bypass or validation behavior being examined.
4. Check that matching rules preserve the intended policy for errors and alternate serving paths. If no cache-status evidence is exposed, report the headers observed and the limits of the inference; do not claim a CDN hit from fast timing alone.

Keep the project's existing delivery guide concise: record the relevant layers, the owner of each rule, effective browser/CDN policies, and verification evidence. If production access is unavailable, complete local checks and state the outstanding hosted verification. Testing does not authorize a production deployment, cache purge, or provider-rule change.

Provider documentation and RFC reviewed: 2026-09-23. These are conceptual examples without executable configuration; no provider integration was tested while authoring this reference. Recheck provider-specific behavior against the consuming project's deployment when implementing it.
