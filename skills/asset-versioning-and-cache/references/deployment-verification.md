# Deployment Compatibility and Verification

Read this when publication, old-client compatibility, retention, rollback, or related verification is part of the task. Routine changes using an unchanged delivery contract do not need the full review.

## Choose and preserve a compatibility policy

A deployment can leave cached HTML or an already open page referring to previous assets. HTML revalidation only helps when that document is requested again. An old tab may request a previously unloaded chunk much later; switching releases atomically does not update that tab.

Use the project's chosen strategy. When introducing one, recommend making new assets available before exposing their references and retaining prior referenced assets for a defined compatibility window. Account for accepted HTML freshness, open-client lifetime, rollback needs, and hosting capabilities. Do not infer an adequate retention period solely from the one-year asset-cache default: a client might never have cached a particular old asset.

Possible approaches include keeping previous content-derived files, retaining addressable release directories, or using an existing provider's deployment-version routing. Choose a mechanism appropriate to the project rather than requiring all of them. If the project deliberately serves only current output, keep its accepted limitation explicit and evaluate its recovery behavior within the requested scope.

Recovery may offer an update or reload when an old dependency is unavailable. Avoid unconditional reload loops and unexpected loss of unsaved work. Recovery does not establish uninterrupted compatibility; do not silently replace retention requirements with a reload handler.

The failure mode also appears in [Vite's production load-error guidance](https://vite.dev/guide/build.html#load-error-handling): removing old chunks can break later imports from earlier pages. This is relevant beyond Vite and does not require installing it.

## Keep build and publication responsibilities aligned

- Verify that the build's generated references and the published artifacts belong to the same release, including lazy dependencies and CSS-linked resources. Use the existing manifest or dependency graph where available.
- Keep content transformations inside the versioned pipeline. Post-build substitutions that change published asset content must also update its identity and incoming references; renaming just the leaf file leaves stale consumers.
- Preserve the build tool's intended behavior for inlined and public-directory assets. For example, [Vite's asset handling](https://vite.dev/guide/assets.html) distinguishes imported resources in the build graph from fixed-name public files. Adapt to the installed tool rather than copying configuration from another framework or version.
- Cleanup must follow the adopted retention rule. A directory listing of the newest build does not establish that every older client has stopped referencing previous files. Rollback needs its matching assets and references, not merely the old HTML.

## Select checks for the affected behavior

| Change | Useful evidence |
| --- | --- |
| Hashing or asset graph | Rebuild identical inputs with the same toolchain, then change one input. Confirm that changed output gets new URLs and dependent references resolve; explain avoidable churn in unchanged assets. |
| Headers or routing | Fetch representative successful assets, root/HTML, fixed files, and a missing asset through the affected serving path. Check status, content type, body, and effective cache headers. A missing script must not return the app's HTML with status 200. |
| Validators | Where validation is supported, reuse the returned validator and check unchanged versus changed content. Confirm the expected `304` or updated response instead of assuming a header name proves correct handling. |
| Deployment compatibility | Preserve an old page or its reference list, publish a second build in an isolated test environment, and request an old asset not already cached. Exercise delayed imports when present. Expect retention or the explicitly accepted failure/recovery behavior. |
| Publication ordering or rollback | Verify that referenced assets exist when their document becomes reachable and that the chosen rollback restores a compatible set. Use local or staging evidence when production changes are not authorized. |
| Cache reuse | Exercise repeat loads with caching enabled. Separate initial transfer, reuse without a request, and validated reuse; developer tools with caching disabled cannot demonstrate the intended reuse. |

Run the applicable checks through existing project tools. Do not require every scenario for each task or create a new test framework solely for this skill. Local HTTP checks establish local behavior; claim CDN, browser, or service-worker behavior only when that layer was actually observed.

Documentation reviewed: 2026-09-23. The Vite links explain concepts; this reference contains no version-specific API examples and claims no tested Vite version or hosting integration. Consult documentation matching the consuming project's resolved toolchain when implementing changes.
