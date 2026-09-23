# Platform Delivery

Read the sections relevant to the target consumers. These are integration criteria, not a mandatory cross-platform asset bundle.

Documentation reviewed: **2026-09-23**. The HTML example targets the WHATWG Living Standard; it is illustrative and documentation-checked, not browser-tested. No particular framework or SDK version is required. Recheck platform requirements against the consuming project's supported versions before implementing an export matrix.

## Web images and density

For a fixed logical size, generate raster candidates with identical framing and wire them with density descriptors. This example displays one 24 px symbol using independently rendered 24, 48, and 72 px images:

```html
<img
  src="/assets/mark.png"
  srcset="/assets/mark.png 1x, /assets/mark@2x.png 2x, /assets/mark@3x.png 3x"
  width="24"
  height="24"
  alt="Example brand"
>
```

Use the project's asset imports or generated URLs instead of introducing fixed paths when it already hashes assets. Match alternative text to the real context, including empty text for decorative or redundant images. Keep the layout at the intended logical size.

For images whose layout width varies, use width candidates and an accurate `sizes` expression instead of treating viewport size and pixel density as the same thing. Do not mix width and density descriptors within one source set. Verify the selected source and displayed dimensions under fresh browser contexts; browser selection can also depend on caching and user-agent policy. See the [HTML image source-set rules](https://html.spec.whatwg.org/dev/images.html).

An existing native SVG image generally removes the need for raster density variants on that surface. Preserve accessible semantics and styling contracts when choosing inline SVG, an external image, or an existing icon component.

## Favicons and touch icons

- Use small-size artwork for the SVG favicon, and generate the fallbacks required by supported browsers. Raster sizes such as 16, 32, and 48 px are useful starting candidates, not a complete universal requirement. A fallback ICO may contain multiple independently rendered layers.
- For `rel="icon"`, declare bitmap `sizes` that match actual pixels, or `sizes="any"` for a scalable icon. These values describe the resource; they are not `srcset` density descriptors. Do not invent `favicon@2x` links and assume the browser understands the suffix. See the [HTML icon link rules](https://html.spec.whatwg.org/multipage/links.html#rel-icon).
- Treat touch and installed-app icons as separate consumers with their own dimensions, background, and mask requirements. Use the project's platform/framework metadata and current official guidance; a tightly cropped transparent favicon is not automatically suitable.
- Verify the emitted head/manifest links, served bytes, content types, and the actual browser tab or installed shortcut. Account for favicon caches when checking a changed image. A correct image inside the page does not prove the browser chrome uses it.

## Native desktop surfaces

Distinguish executable icons, shortcuts, window icons, notification-area symbols, installer resources, and app-list entries. Sharing one source is appropriate only when the required treatment is the same.

For Windows, choose ICO layers or packaged asset variants according to the actual application model and loader. Include the required physical sizes for supported display scales rather than assuming the loader always chooses the largest image. Check embedded layers and the native loading result; producing an extra size does not prove the OS uses it. The [Windows icon construction guide](https://learn.microsoft.com/en-us/windows/apps/design/iconography/app-icon-construction) distinguishes scale-based assets from exact target sizes.

For Apple platforms, preserve the existing asset catalog and build-tool conventions, including vector support where available. Raster scale factors describe pixels per logical point; supply only the variants relevant to supported devices. See [Apple image scale factors](https://developer.apple.com/design/human-interface-guidelines/images) and [asset catalog image import](https://developer.apple.com/documentation/xcode/adding-images-to-your-xcode-project).

When a tray/menu-bar surface uses a template or monochrome mask, preserve the intended alpha silhouette. For fixed-color artwork, validate both themes without adding unnecessary runtime switching. If density or theme changes require runtime resource reselection, exercise that behavior or document its limits.

## Mobile and installed web apps

- Generate the inputs expected by the existing mobile framework or native asset pipeline. Let that pipeline own its generated platform trees; do not manually patch derived resources that the next build replaces.
- Android adaptive icons use separate background/foreground layers and can use a monochrome layer for themed icons. Keep essential artwork inside the documented safe region and preview multiple masks. Do not copy a tray icon's crop or bake a launcher mask into every layer. Consult [Android adaptive icon guidance](https://developer.android.com/develop/ui/compose/system/icon_design_adaptive) for the target platform.
- For web manifests, distinguish ordinary icons from `maskable` and `monochrome` artwork; set `purpose` only when the asset meets that contract. Maskable icons keep essential content within the central circle of radius 40% of the smaller image dimension, with a background that tolerates cropping. See the [Web App Manifest mask rules](https://www.w3.org/TR/appmanifest/#icon-masks).

Safe regions and export dimensions belong to their platform; a PWA maskable icon, Android foreground, and Apple app icon are not interchangeable simply because all are square.
