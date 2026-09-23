---
name: svg-icon-assets
description: "Create, update, or review application icons, UI symbols, logos, tray icons, favicons, touch icons, and their asset pipelines. Use SVG masters, artwork suited to small sizes, appropriate 1x/2x/3x raster outputs, and reproducible production-build optimization."
---

# SVG Icon Assets

Use editable SVG artwork as the source of truth and generate the raster formats each consumer needs. Design for the actual display size, then minimize production bytes without compromising that design.

Preserve the project's icon library, visual language, framework, and build conventions. Reuse existing vector components or symbols where appropriate. This skill does not require replacing an icon system, tracing photographs into SVG, or optimizing unrelated site behavior.

## Establish the asset contract

- Read the project's asset guide before changing artwork or generation. Locate canonical sources, generators, generated files, references, and production build entrypoints. If the guide is missing, record the relevant decisions in a concise document following the project's conventions; no particular filename is required.
- Map each affected surface to its source variant, logical display size, required physical pixel dimensions or native vector format, theme behavior, transparency/masking rules, and actual consumer. Include fallbacks and native resources when relevant.
- Preserve editable originals, provenance, and licenses. Keep generated files distinct from masters; make changes in the source or generator instead of hand-editing generated rasters. An SVG containing an embedded bitmap does not make that bitmap resolution independent.
- Keep exploratory concepts outside production inputs. Promote a concept only after the selected direction is recorded in the asset guide and its outputs are rebuilt and verified. Reuse an already selected direction; routine regeneration does not require a new design-selection step.
- Inspect resolved tool versions and existing optimization stages. Extend the current pipeline and avoid duplicate encoders or dependencies without a concrete benefit.

## Design for small surfaces

- Begin with the smallest relevant use, often a 16 px favicon or a small tray symbol. Evaluate recognizability and meaning there before judging a large preview. When depicting a real object or device, retain its defining geometry without importing product-specific details into unrelated artwork.
- Prefer a clear silhouette, open gaps, deliberate contrast, and enough edge clearance for antialiasing. Simplify fine details, thin lines, tiny lettering, and fragile gradients before adding stroke weight or resolution.
- Create separate optical SVG variants when one drawing cannot serve tiny and large uses. Adjust spacing, stroke coverage, optical centering, and pixel-grid alignment at the intended size; do not stretch the object to fill a square or crop away necessary padding.
- Use real alpha transparency for cutouts. Do not paint holes in the color of a preview background. Distinguish transparent symbols, artwork on a background plate, maskable icons, and monochrome silhouettes.
- Decide whether colors are fixed or depend on the theme. Check light and dark surroundings in both cases; implement theme selection or native template behavior only when the chosen artwork needs it. Do not assume external SVG files inherit a page's CSS color.
- Review at the intended logical size on light and dark backgrounds and at supported display scales. Enlarged pixel views help diagnose defects but do not replace normal-size review. Extra pixels cannot repair an ambiguous symbol.

## Generate the resolutions consumers use

- Prefer delivering SVG directly where the consumer supports it and rendering is suitable. A vector consumer does not need duplicate `@2x.svg` or `@3x.svg` files.
- For raster consumers, derive physical dimensions from logical dimensions and density: a 24 by 24 logical-pixel image needs 24, 48, and 72 pixel outputs for 1x, 2x, and 3x. Include the scales the target supports; also account for platform-specific or fractional scales when relevant.
- Render each required raster from its appropriate SVG variant at the target dimensions. Never enlarge an existing small PNG or use one large PNG as the canonical source for every size. Ensure the SVG renderer has sufficient rasterization resolution; any supersampling must be deliberate and checked against direct rendering.
- Select optical artwork by intended surface and display size as well as pixel dimensions. A 32 px file for a 16 px symbol at 2x need not use the same drawing as a 32 px symbol at 1x.
- Wire variants into real consumers: web density descriptors, native asset catalogs, framework conventions, or multi-image icon containers. A filename suffix alone does not establish density support. Avoid redundant files and do not multiply fixed platform-required dimensions by 2 or 3 automatically.
- Read [platform delivery](references/platform-delivery.md) when choosing favicon fallbacks, web density markup, native resources, or mobile masking rules. Verify current requirements for the project's actual browser and OS targets instead of copying a universal export list.

## Make final optimization part of the production build

The normal production build or release-packaging command must generate and optimize the affected assets automatically from a clean checkout. A manually run optimizer or previously optimized committed file is not sufficient integration.

- Include the strongest validated optimization settings in the production path for both emitted SVGs and generated rasters. Keep development fast with caching or cheaper settings when useful, while preserving dimensions, source selection, and appearance. Do not move expensive static-asset optimization into the application runtime.
- Optimize generated copies, retaining editable masters. Maximize compression effort within supported formats and the visual contract; maximum optimization does not mean maximum lossy compression. Use lossless optimization by default for small icons and brand artwork. Treat lossy quantization, visual simplification, and format changes as distinct decisions requiring appropriate visual validation within the task's scope.
- Read [production optimization](references/production-optimization.md) when implementing or changing the pipeline. Measure candidates and keep the smallest valid representation for the actual delivery path; a successful encoder run can still produce a larger file.
- Finish byte-changing operations before content hashing, URL generation, manifests, resource embedding, or package signing. Preserve required fixed filenames and the project's caching conventions. Package only current consumer-required outputs and exclude source archives and review sheets.
- Make outputs reproducible with recorded settings and locked build dependencies. Include source variants, generation settings, and tool versions in cache invalidation. Keep cleanup limited to generator-owned outputs, and fail the production build on required generation or validation errors.

## Verify the delivered result

Run the relevant production build and existing asset checks after changes. For a pipeline change, also verify a clean rebuild, stable output for unchanged inputs, and regeneration when a source or setting changes. If generated files are committed for tooling requirements, detect stale copies as part of the build or CI.

Check these distinct outcomes:

| Check | Evidence |
| --- | --- |
| Artifact correctness | Required files exist and decode; dimensions, alpha, format, container layers, and references match the asset contract. |
| Visual quality | Actual-size comparisons on light/dark backgrounds and supported 1x/2x/3x scales show readable shapes, clean edges, and no clipping. |
| Optimization fidelity | Decoded-pixel or rendering comparisons support the claimed losslessness; any intended visual differences are identified separately. |
| Delivery | Built pages or packages consume the intended variants. Check served URLs, MIME types, density selection, masks, and native resource loading as applicable. |
| Measured size | Compare final asset or package bytes and relevant compressed transfer sizes. Report measured savings without claiming unmeasured load-time improvements. |

Browser emulation and contact sheets do not establish native-device equivalence. Inspect the target browser chrome, installed tray, launcher, or other native surface when available; report any remaining platform check explicitly.

Update the asset guide with the selected sources, output matrix, production generation/check commands, and meaningful limitations. Report what changed, what was verified, and any unverified visual or native behavior.
