---
name: svg-icon-assets
description: "Create, change, or review icons, logos, favicons, tray/touch icons, or their asset pipelines. Unrelated UI styling and general image work do not trigger this skill."
---

# SVG Icon Assets

Use editable SVG artwork as the source of truth and generate the raster formats each consumer needs. Design for the actual display size, then minimize production bytes without compromising that design.

Preserve the project's icon library, visual language, framework, and build conventions. Reuse existing vector components or symbols where appropriate. This skill does not require replacing an icon system, tracing photographs into SVG, or optimizing unrelated site behavior.

Respect documented project exceptions and choices already resolved by the user. If this guidance conflicts with an established asset requirement, show the unresolved conflict and ask whether to adjust that requirement or record a scoped exception before the disputed change. Do not invent an exception; record an agreed one in the project's asset guide or instructions with its scope and reason. Ordinary platform-specific output choices are not policy conflicts.

## Establish the asset contract

Scope ordinary use to affected artwork, surfaces, and pipeline stages. Reuse the asset contract and exceptions without reopening approved artwork. For a review or explanation, inspect the requested assets and evidence; regenerate, build, or change files only when needed by the task. For implementation, follow the relevant artwork, delivery, and pipeline steps below.

- Read the relevant asset-guide sections and locate affected sources, generators, outputs, consumers, and build entrypoints. During implementation, record missing asset-contract decisions in a concise guide following project conventions. A review-only task can report missing contract information without creating a document.
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

Reuse an established, validated pipeline for ordinary artwork changes. Read [production optimization](references/production-optimization.md) when creating or changing generation, optimization, or packaging, or investigating evidence that the pipeline is inadequate. Do not retune encoders or repeat candidate comparisons solely because another icon changed.

## Verify the delivered result

After implementation changes to assets or their delivery, run the relevant production build and existing asset checks. For a pipeline change, also verify a clean rebuild, stable output for unchanged inputs, and regeneration when a source or setting changes. If generated files are committed for tooling requirements, detect stale copies as part of the build or CI. A documentation-only edit follows documentation checks instead.

Check the outcomes relevant to the change. Compression comparisons apply when optimization changes or savings are claimed; native/browser checks apply to affected consumers rather than every supported platform:

| Check | Evidence |
| --- | --- |
| Artifact correctness | Required files exist and decode; dimensions, alpha, format, container layers, and references match the asset contract. |
| Visual quality | Actual-size comparisons on light/dark backgrounds and supported 1x/2x/3x scales show readable shapes, clean edges, and no clipping. |
| Optimization fidelity | Decoded-pixel or rendering comparisons support the claimed losslessness; any intended visual differences are identified separately. |
| Delivery | Built pages or packages consume the intended variants. Check served URLs, MIME types, density selection, masks, and native resource loading as applicable. |
| Measured size | Compare final asset or package bytes and relevant compressed transfer sizes. Report measured savings without claiming unmeasured load-time improvements. |

Browser emulation and contact sheets do not establish native-device equivalence. Inspect the target browser chrome, installed tray, launcher, or other native surface when available; report any remaining platform check explicitly.

Update the asset guide when selected sources, the output matrix, generation/check commands, or meaningful limitations change. Leave an accurate guide unchanged. Report what changed, what was verified, and any unverified visual or native behavior.
