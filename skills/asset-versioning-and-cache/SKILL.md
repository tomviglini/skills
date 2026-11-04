---
name: asset-versioning-and-cache
description: "Implement or review static-asset versioning, HTTP cache policies, and asset compatibility across deployments. Use for asset-pipeline or delivery changes and stale-asset failures; excludes ordinary artwork, content, or styling edits using unchanged delivery."
---

# Asset Versioning and Cache

Give changing static assets new content-derived URLs and make their delivery policy explicit. Reuse the project's build tools and hosting conventions. Keep the skill independent of frameworks, hosting providers, and hash algorithms.

For public assets with immutable content-derived URLs, recommend `Cache-Control: public, max-age=31536000, immutable` by default: one year of freshness. This is the collection's recommended policy, not an HTTP requirement. The root page, HTML, and other mutable URLs have a project-specific policy; do not impose a shared lifetime on them.

## Scope adoption and ordinary use

- During initial adoption or an explicit delivery audit, trace the relevant source assets, build outputs, URL references, response headers, and deployment cleanup. Reconcile existing instructions and guides through the project's adoption procedure. Preserve project-specific values, exceptions, and historical ADRs; replace only equivalent generic instructions with the skill reference. Do not create a separate installer or adoption report.
- During ordinary use, reuse the adopted policy and inspect only the affected pipeline, responses, or deployment behavior. A content or artwork edit using an established delivery pipeline does not trigger another cache audit. Read supporting references only for the work that needs them.
- For review or explanation, report findings without changing runtime behavior. Installation alone does not authorize a bundler migration, URL migration, deployment, or cache purge. Complete implementation and relevant checks when that work is requested.
- Respect explicit exceptions and resolved choices. If a requirement conflicts with an existing project rule, show the exact conflict and ask whether to change the rule or record a scoped exception before the disputed change. Do not invent exceptions or ask again about settled choices. Different lifetimes for mutable HTML are ordinary project decisions, not conflicts.

## Version assets through the production pipeline

- Prefer the existing bundler's content hashing and reference rewriting for scripts, styles, fonts, images, and other affected static resources. Integrate this into the normal production build; avoid manual renaming, random versions, and timestamps that invalidate unchanged assets on every build.
- Version the emitted content and its dependencies after relevant transformations. Let the tool manage its asset graph rather than assuming every hash must equal a raw source-file digest. For content-versioned assets, changed output must receive a new URL; never overwrite a published immutable URL with a different representation. A toolchain change can legitimately change generated names.
- Make consumers resolve the generated URLs through imports, manifests, or the build system's equivalent. Cover affected HTML, CSS URLs, module imports, lazy chunks, and metadata. Assets copied unchanged from a public directory often keep fixed names; classify them by their actual output instead of assuming they were fingerprinted.
- Keep required fixed URLs, public page routes, and externally consumed paths stable. Give mutable fixed URLs their own policy. Inlined resources inherit their containing document or bundle's version and cache behavior; do not generate separate files solely to apply this skill.
- Preserve the intended deployment base and asset origin. A preview should load its matching artifacts, including when the project intentionally uses a separate asset host. Do not hard-code a production asset location into unrelated previews or force same-origin delivery where a versioned CDN is already intentional.

## Apply the correct policy to each response

| Resource | Policy |
| --- | --- |
| Public static asset at a verified immutable, content-derived URL | Recommend one year plus `immutable`; preserve an explicit project override. |
| Root page, HTML, mutable fixed filenames, or runtime configuration | Use the project's freshness requirement: validation on reuse, zero freshness, or an accepted positive lifetime. Keep these separate from the immutable-asset rule. |
| Missing asset or error response | Never inherit successful assets' long immutable policy. Prefer `no-store` for transient asset misses unless a deliberate negative-cache policy exists. |
| Personalized or API response | Keep its existing dedicated policy; exclude it from blanket public-asset rules. Changing API caching is a separate task. |

Apply the classification to actual responses, including alternate serving paths such as embedded files and filesystem delivery when both are supported. An asset-looking filename or extension alone does not prove that the response is a successful immutable asset. Missing scripts and styles must not become successful HTML fallback responses.

Read [cache policy](references/cache-policy.md) when choosing or changing headers, validators, mutable-resource freshness, or cache-layer configuration. It distinguishes `no-cache`, `max-age=0`, and `no-store`; they are not interchangeable.

For production delivery changes or reviews, establish whether the CDN actually caches each affected resource, its effective freshness lifetime, and the rule that determines them. Then verify the policy delivered to the browser, which has its own cache. Read [production delivery](references/production-delivery.md) for header precedence and verification through the published URL. Seeing the intended browser header does not prove the CDN uses it; reuse a verified deployment contract for unrelated work.

## Keep versions usable across deployments

Define what happens when an old page requests an asset after a newer release is published. Include already open tabs and delayed imports when present. Content hashes prevent stale content at a changed URL; they do not keep deleted files available.

Read [deployment compatibility and verification](references/deployment-verification.md) when changing publication, retention, rollback, or cache lifetimes that affect older clients, and when investigating missing assets after a release. Reuse a documented retention or recovery policy. Preserve an explicitly accepted current-release-only limitation and report it accurately rather than silently promising compatibility or creating a retention system during adoption.

## Verify and record affected behavior

- For implementation, build with the project's production path and check the affected emitted URLs, references, and HTTP responses. Header-only changes need response checks, not an unrelated full pipeline rebuild. A development server's behavior does not establish production delivery.
- For changes to hashing or reference generation, check a repeat build with unchanged inputs and a build with one relevant input changed. Confirm useful reuse and invalidation without requiring unrelated assets to change names. Use the reference's deployment scenarios only when those behaviors are affected.
- Reuse existing checks and evidence for unaffected paths. Review-only and documentation-only work do not require running a deployment or constructing a new application harness.
- Update the existing asset or deployment guide when the actual policy, commands, exceptions, or known limitations change. Keep exact lifetimes, paths, tool versions, and hosting details in the project. Preserve decision history and distinguish static review, local production checks, and observed hosted behavior. Report verification limits without claiming measured performance improvements from header changes alone.
