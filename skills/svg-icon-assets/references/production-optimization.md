# Production Optimization

Use this reference when implementing asset generation or optimization. The production build owns the final optimization; optional audit commands may compare candidates but must not be the only way to obtain release assets.

## Build integration

Fit these dependencies into the existing build graph:

```text
canonical sources + output matrix + recorded settings
  -> generate target variants and raster sizes
  -> optimize SVG/raster candidates and validate the selected bytes
  -> assemble required icon containers
  -> hash assets and resolve consumer references
  -> bundle/embed/package/sign
  -> verify the delivered artifacts
```

An SVG optimizer may run before rasterization when its rendering fidelity is established. Container compression may be part of assembly. The invariant is that consumers, hashes, and signatures refer to final bytes, and validation examines what is actually shipped.

- Make the normal production command invoke the pipeline in a clean checkout, including every relevant release target. Check CI/deployment entrypoints rather than assuming they invoke a particular package-manager lifecycle hook.
- Put the expensive, validated compression effort in production. Development can reuse cached results or perform cheaper compression; it must still notice changed sources and preserve geometry, alpha, and variant selection.
- Keep build tools available in the build environment without adding them to the application runtime. Reuse compatible installed dependencies; lock any newly adopted build dependency according to project conventions.
- Cache by source content, referenced inputs, output dimensions, theme/optical variant, settings, and encoder versions. A warm local cache or committed PNG must not hide a missing generation step. Avoid timestamps and machine-specific metadata in emitted assets.
- Distinguish an unsuccessful optimization candidate from a tool failure. A valid candidate that is larger can be rejected in favor of the baseline. A required encoder failure, missing source, corrupt output, or failed validation must stop the build, not silently ship a stale file.

## SVG optimization

Retain the editable master and optimize its emitted copy. Preserve the `viewBox`, aspect ratio, necessary padding, alpha, masks, clipping, gradients, filter bounds, and color behavior. Protect IDs used by fragment URLs, CSS, animation, accessibility relationships, or source extraction. Check for collisions when combining independently optimized inline icons.

Do not assume an optimizer's default preset is visually lossless. Path conversion, numeric rounding, transform rewriting, and cleanup can change tiny strokes or remove a referenced element. Configure transformations for the asset's usage and compare renders at its real sizes on both backgrounds. More decimal places alone do not disable other geometry-changing transformations. [SVGO's default preset](https://svgo.dev/docs/preset-default/) illustrates why plugin selection needs review.

Editor metadata and unused definitions are candidates for removal when they are truly unnecessary. Preserve required attribution and accessible semantics. SVGs that depend on fonts or external images need reproducible inputs; do not silently substitute fonts or claim embedded rasters scale as vectors.

## Raster and container optimization

- For small icons with sharp edges and alpha, start with lossless encoding in a format the consumer supports. Evaluate high compression effort and filter strategies; choose measured output rather than assuming a maximum knob always wins for every image.
- Palette reduction is acceptable as lossless only if it preserves the required pixel data exactly. Color quantization, reduced alpha precision, lossy WebP/AVIF, and visual reconstruction are separate changes. Do not label them lossless because differences are hard to see in a large preview.
- Preserve color interpretation, alpha, and required metadata. Normalize to the intended color space before removing a profile when appropriate, then verify appearance. Remove density metadata only when consumers do not rely on it. Do not apply an ordinary static-PNG recipe blindly to animation, unusual bit depths, or unknown metadata.
- Compare decoded dimensions and RGBA when claiming exact raster equality, including RGB under transparent pixels. If only composited appearance is equivalent, state that narrower result. Composite on light and dark backgrounds to detect halos and broken cutouts.
- Compare the same pre-encoding render when validating compression. A source redesign or a changed rasterizer is a different cause of pixel changes and should be evaluated separately.
- Assemble ICO or other required containers from validated target-size images without an unintended second resize or quantization. Check each embedded layer and native decoder compatibility, not only the standalone PNGs.

For an existing Sharp pipeline, the [output options documentation](https://sharp.pixelplumbing.com/api-output/) describes PNG compression, filtering, palette, and metadata behavior. Inspect the installed version's API before setting options; this reference does not prescribe Sharp or a universal encoder configuration.

## Choose candidates using relevant measurements

For each affected asset or delivery unit, record baseline and candidate bytes, dimensions, format, settings, and fidelity results. Keep the smallest candidate that satisfies compatibility and the visual contract. If every valid recompression increases size, retaining the original generated bytes is a valid build result; do not ship a larger file just to call it optimized.

Measure raw file or container size for native packages. For web delivery, also compare the actual relevant gzip/Brotli representation under matched settings when applicable. For inline or data-URL images, measure the complete compressed HTML/CSS delivery unit: summing individually compressed images or comparing Base64 character counts does not establish transfer savings. Verify which encoding the server actually sends before claiming production transfer savings.

Avoid universal format rankings. SVG can eliminate bulky raster content, but complex filters can increase rendering cost; a vector approximation of an existing bitmap is an artwork change. WebP/AVIF may suit other imagery, but a favicon, OS loader, or transparency contract can require a different format. Keep photographic masters as rasters and preserve originals when broader image optimization is explicitly in scope.

Keep format/compression work distinct from loading priority, lazy loading, layout changes, or cache-policy changes. Those alter behavior and require their own task scope and evidence. Reduced bytes alone do not establish faster rendering or a better loading metric.

## Validation status and tool versions

Documentation reviewed: **2026-09-23**, using the official SVGO and Sharp sources linked above. These are tool-selection and validation criteria, with no executable library-specific examples or tested version matrix. They do not claim compatibility with every release.

When adding executable configurations to a consuming project or extending this reference with code, record the API target, resolved versions actually tested, documentation review date, sources, and validation results. Adapt to the project's installed versions instead of requiring upgrades to match an example.
