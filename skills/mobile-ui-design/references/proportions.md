# Proportions

Read this reference when choosing or reviewing spacing, touch targets, control sizes, type, corners, colors, or the size range on a mobile screen. The values are references, not a design system: they keep a project's screens consistent with each other and with the platform. A project that needs other values records them, with the reason, in its UI guide; the recorded values then apply to that project.

Sources were reviewed on 2026-10-27. They are listed at the end; the values are summarized here, not copied. Units: dp on Android, text included: the app's sizes are fixed, so it does not use sp, which follows the system's font size. Points on iOS, CSS pixels for a web UI on a phone.

## Where the values come from

- **The platform** sets what an app must meet and what feels at home. On Android, the core app quality guidelines require 48 dp targets and contrast minimums. On iOS, Apple's guidelines set 44 pt controls and a 17 pt body.
- **Material 3** gives the proportions of Android's own components: its published tokens (buttons, fields, lists, dialogs, sheets) and its layout breakpoints and margins. A project with its own components keeps their look but takes the sizes from here where it has none of its own.
- **Accessibility** sets the minimums that do not move: WCAG 2.2 contrast for text and for controls and their states, target sizes, and an alternative to dragging.
- **Practitioners' research** covers what the numbers miss, such as reaching controls with the thumb and confirming only what cannot be undone ([patterns](patterns.md)).

## Reference values

| Topic | Reference | Notes |
| --- | --- | --- |
| Spacing scale | 4, 8, 12, 16, 24, 32 dp | Steps of 4. Values like 2 or 6 only as optical corrections. |
| Page margins | 16 dp below 600 dp wide, 24 dp from 600 dp | Material's compact and medium window classes. Insets add to the margins; they do not replace them. |
| Between adjacent targets | 12 dp | Buttons in a row, a field and its action. Material uses 8; 12 keeps buttons visibly apart. |
| Between rows | 12 dp | Rows of one card or form. |
| Card edge to content | 16 dp | |
| Between cards or sections | 16 to 24 dp | More space between groups than inside them. |
| Touch target | 48 × 48 dp on Android, 44 × 44 pt on iOS | Android's core app quality requires 48 dp. The visual can be smaller (a 40 dp button, a 24 dp icon) with a larger hit area, as Material's components do. |
| Buttons | 40 dp, up to 48 or 56 for a screen's main action | Material 3's small, default button is 40 dp with a 48 dp target; medium is 56 dp. iOS regular buttons are 44 pt. |
| Text fields | 56 dp in Material, 44 pt or more on iOS | 48 dp is a common compact value for custom fields; keep one height per form. |
| List rows | 56 dp one line, 72 two lines, 88 three lines | Leading and trailing padding 16 dp. |
| Navigation bar | 64 to 80 dp plus the bottom inset | Three to five destinations. |
| Body text | 16 dp regular | Main reading text and the text typed into fields. 17 pt on iOS. |
| Secondary text | 14 dp regular | Descriptions, second lines of rows. |
| Button and tab labels | 14 dp medium | |
| Captions | 12 dp | The smallest text, badges and tab labels included. Material's smallest label (11) and iOS's 11 pt minimum are smaller; a project that uses them records why. |
| Card and dialog titles | 16 to 20 dp semibold, 24 dp for dialogs | Material's title medium (16) and headline small (24) are the references. |
| Screen titles | 22 to 28 dp | Title large (22) in a top app bar, headline medium (28) for a large title. |
| Weights | Regular and semibold (600); bold only for the screen's title | Material uses regular and medium, iOS regular and semibold with bold large titles. One emphasis weight keeps the hierarchy in size and color instead of in heavier type. |
| Line height | About 1.25 to 1.5 times the size | |
| Corner radius | 4, 8, 12, 16, 28 dp and full | Material's shape scale: fields 4, snackbars 4, chips 8, cards 12, large cards and buttons in square style 16, dialogs and sheet tops 28, pill buttons and switches full. Nested shapes stay concentric: inner radius = outer radius - gap. |
| Text contrast | At least 4.5:1 | 3:1 is enough only for large text: 24 dp regular or 19 dp bold (WCAG's 18 pt and 14 pt bold). |
| Control edges and state markers | At least 3:1 | Field outlines, switch tracks, the fill or edge of the selected segment, against the adjacent colors. Disabled controls are exempt. |
| Size range | 393 to 480 dp | The widths every screen is designed and checked at; outside them the app scales the nearest end. Phones start at about 360 to 412 dp. |
| Line length | 45 to 75 characters | For paragraphs; on phones this usually holds by itself. |

## Applying them

- **Pick the app's own values once** from the ranges above and keep them as named tokens next to the shared components (spacing, radii, type styles, colors). Screens use the tokens, never ad hoc numbers.
- **Targets:** where the design calls for a small visual, such as a value chip or an icon, give it a hit area of 48 dp (`hitSlop` in React Native, `minimumInteractiveComponentSize` in Compose) and check that neighbours do not overlap.
- **Theme colors:** check every foreground against every background it is drawn on, in each theme, and fix the theme rather than the screen. Translucent colors are measured after blending.
- **Selected states** are distinct by more than color alone: fill plus a check, an edge, or a weight change.
- **Changing numbers** use tabular figures so digits do not shift as they change.
- **Deviations:** when the platform, the library, or the owner's choice calls for another value, record it in the project's UI guide with the reason. Do not mix two values for the same role in one app.

## Size range

Fixed sizes and one range of widths replace following the system's font size: the screens are designed and checked where they run, and every phone shows one of them, scaled.

- **The range:** 393 to 480 dp by default. At the narrow end, a row of four buttons or a long label beside its value still fits at the reference sizes above; the wide end keeps cards and lines from spreading. A project records its own range, with the reason, after capturing its screens across widths.
- **Fixed type:** text, icons and inputs ignore the system's font size (in React Native, one shared `Text` and the inputs with `allowFontScaling={false}`; on iOS, fixed sizes instead of Dynamic Type styles). The system's own interface (status bar, keyboard, permission prompts) still follows it.
- **Scaling:** the app lays every screen out at a width inside the range and scales it to fill the phone's width, with the height divided by the same scale. Below the range everything shrinks with it, targets included; above it, everything grows. Each native window, such as a modal, scales its own content; insets, the keyboard and window measurements come in the phone's units and are divided by the scale ([React Native](react-native.md#scaling-to-the-size-range)).
- **Size setting:** the starting width is the phone's own, from its Display size, kept inside the range. An in-app setting, such as a slider from Larger to Smaller with a typed value and a way back to the phone's size, can replace it; the choice is saved and applies to every screen at once. It is how someone makes the app larger: up to the range's narrow end, and further with the system's magnification.
- **Windows wider than a phone:** from 600 dp Android ignores the orientation lock, so a phone app also opens on tablets and in landscape, scaled from the wide end. A project that supports those windows records their layout instead.
- **Rows that do not fit** at the narrow end wrap or stack, and long names wrap to two lines or truncate with the full text reachable. Nothing is designed below the range: no phone shows it.

## Sources

| Source | What it gives | Reviewed |
| --- | --- | --- |
| [Android: Core app quality guidelines](https://developer.android.com/docs/quality-guidelines/core-app-quality) | 48 dp targets, contrast, content descriptions, Back, state, permissions, themes | 2026-10-27 |
| [Android: Make apps more accessible](https://developer.android.com/guide/topics/ui/accessibility/apps) | Contrast thresholds, 48 dp targets, content descriptions | 2026-10-27 |
| [Android: Window size classes](https://developer.android.com/develop/ui/compose/layouts/adaptive/window-size-classes) | Width breakpoints at 600, 840, 1200, and 1600 dp | 2026-10-27 |
| [Material 3: Breakpoints](https://m3.material.io/foundations/layout/breakpoints/medium) | 16 dp compact and 24 dp medium margins | 2026-10-27 |
| [Compose Material 3 tokens](https://github.com/androidx/androidx/tree/androidx-main/compose/material3/material3/src/commonMain/kotlin/androidx/compose/material3/tokens) | Button, field, list, navigation bar, slider, switch, dialog and sheet sizes, the shape scale, and the type scale (tokens v0_11_0) | 2026-10-27 |
| [Apple: Accessibility](https://developer.apple.com/design/human-interface-guidelines/accessibility) | 44 × 44 pt controls, spacing around controls, text contrast | 2026-10-27 |
| [Apple: Typography](https://developer.apple.com/design/human-interface-guidelines/typography) | 17 pt body, 11 pt minimum, Dynamic Type sizes | 2026-10-27 |
| [WCAG 2.2: Contrast (minimum)](https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html) | 4.5:1 text, 3:1 large text | 2026-10-27 |
| [WCAG 2.2: Non-text contrast](https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast.html) | 3:1 for controls and state indicators | 2026-10-27 |
| [WCAG 2.2: Dragging movements](https://www.w3.org/WAI/WCAG22/Understanding/dragging-movements.html) | An alternative to dragging, such as typing a slider's value | 2026-10-27 |
| [WCAG 2.2: Target size (enhanced)](https://www.w3.org/WAI/WCAG22/Understanding/target-size-enhanced.html) | 44 × 44 px targets for touch | 2026-10-27 |
