# Cache Policy

Read this when changing HTTP cache rules or diagnosing freshness, validation, or cache-key behavior. Reuse an established policy for ordinary asset work.

## Mutable resources are a project decision

Identify how soon the project needs changes to its root page, HTML, metadata, and other mutable URLs to become visible. Preserve existing decisions; neither immediate revalidation nor a positive freshness lifetime is a universal requirement. If no policy exists, propose one based on that requirement within the task's scope. Ask only when a consequential product choice remains unresolved.

Use the directives with their distinct meanings:

| Response directive | Meaning relevant to this workflow |
| --- | --- |
| `no-cache` | Storage is allowed, but reuse requires successful validation. |
| `max-age=0` | No positive freshness interval; this alone does not prohibit every form of stale reuse. |
| `must-revalidate` | Once stale, reuse requires successful validation. |
| `no-store` | Instructs caches not to store this exchange; it is not an instruction to purge every older copy. |
| `private` | Prevents shared-cache storage while allowing private-cache storage under the remaining rules. |
| `s-maxage` | Sets shared-cache freshness separately, overriding `max-age` there and requiring validation after it becomes stale. |

These distinctions follow [RFC 9111, response directives](https://www.rfc-editor.org/rfc/rfc9111.html#section-5.2.2). Use the server's supported `ETag` or `Last-Modified` handling for validation where appropriate; a `304` avoids resending the body but still requires a request.

For example, a project requiring validation on every reuse can select `no-cache`; one specifying zero freshness with required validation after staleness can select `max-age=0, must-revalidate`. A public page whose owner accepts delayed updates can use a positive `max-age`. Record the chosen lifetime and trade-off locally instead of turning one example into a collection-wide HTML rule.

## Immutable resources have a recommended default

Recommend `public, max-age=31536000, immutable` for public content-derived asset URLs. Use a different lifetime when the project has an explicit reason. Restrict this rule to representations the publication process will not change at those URLs; a hash-like string is insufficient evidence.

The `immutable` directive promises no representation change during freshness; it does not create freshness by itself or guarantee permanent client storage. Versioned URLs allow changed resources to coexist with their predecessors. See [RFC 8246, immutable responses](https://www.rfc-editor.org/rfc/rfc8246.html#section-2). This skill uses the stronger publication convention of never repurposing a content-derived URL.

For production header precedence, intermediary overrides, and cache-key checks, read [production delivery](production-delivery.md) when that setup is affected. Apply the directives at the layers that actually control delivery, not just in the application configuration.

Standards reviewed: 2026-09-23. This reference supplies abstract policy guidance, not framework configuration or claims of browser/CDN execution. Verify provider-specific behavior against the consuming project's actual setup.
