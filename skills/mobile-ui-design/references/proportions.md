# Proportions

Read this reference when choosing or reviewing spacing, touch targets, control sizes, type, corners, colors, or text scaling on a mobile screen. The values are references, not a design system: they keep a project's screens consistent with each other and with the platform. A project that needs other values records them, with the reason, in its UI guide; the recorded values then apply to that project.

Sources were reviewed on 2026-10-27. They are listed at the end; the values are summarized here, not copied. Units: dp and sp on Android (sp for text, which follows the user's text size), points on iOS, CSS pixels for a web UI on a phone. At the default text size the three are the same size on screen.

## Where the values come from

- **The platform** sets what an app must meet and what feels at home. On Android, the core app quality guidelines require 48 dp targets and contrast minimums, and Android 14 and later scale text up to 200 %. On iOS, Apple's guidelines set 44 pt controls, a 17 pt body, and Dynamic Type.
- **Material 3** gives the proportions of Android's own components: its published tokens (buttons, fields, lists, dialogs, sheets) and its layout breakpoints and margins. A project with its own components keeps their look but takes the sizes from here where it has none of its own.
- **Accessibility** sets the minimums that do not move: WCAG 2.2 contrast for text and for controls and their states, an alternative to dragging, and content that survives larger text.
- **Practitioners' research** covers what the numbers miss, such as reaching controls with the thumb and confirming only what cannot be undone ([patterns](patterns.md)).

## Reference values

| Topic | Reference | Notes |
| --- | --- | --- |
| Spacing scale | 4, 8, 12, 16, 24, 32 dp | Steps of 4. Values like 2 or 6 only as optical corrections. |
| Page margins | 16 dp below 600 dp wide, 24 dp from 600 dp | Material's compact and medium window classes. Insets add to the margins; they do not replace them. |
| Between adjacent targets | 8 dp | Buttons in a row, a field and its action. |
| Between rows | 12 dp | Rows of one card or form. |
| Card edge to content | 16 dp | |
| Between cards or sections | 16 to 24 dp | More space between groups than inside them. |
| Touch target | 48 × 48 dp on Android, 44 × 44 pt on iOS | Android's core app quality requires 48 dp. The visual can be smaller (a 40 dp button, a 24 dp icon) with a larger hit area, as Material's components do. |
| Buttons | 40 dp, up to 48 or 56 for a screen's main action | Material 3's small, default button is 40 dp with a 48 dp target; medium is 56 dp. iOS regular buttons are 44 pt. |
| Text fields | 56 dp in Material, 44 pt or more on iOS | 48 dp is a common compact value for custom fields; keep one height per form. |
| List rows | 56 dp one line, 72 two lines, 88 three lines | Leading and trailing padding 16 dp. |
| Navigation bar | 64 to 80 dp plus the bottom inset | Three to five destinations. |
| Body text | 16 sp regular | Main reading text and the text typed into fields. 17 pt on iOS. |
| Secondary text | 14 sp regular | Descriptions, second lines of rows. |
| Button and tab labels | 14 sp medium | |
| Captions | 12 sp | The smallest text, badges and tab labels included. Material's 11 sp label size and iOS's 11 pt minimum are smaller; a project that uses them records why. |
| Card and dialog titles | 16 to 20 sp semibold, 24 sp for dialogs | Material's title medium (16) and headline small (24) are the references. |
| Screen titles | 22 to 28 sp | Title large (22) in a top app bar, headline medium (28) for a large title. |
| Weights | Regular and semibold (600); bold only for the screen's title | Material uses regular and medium, iOS regular and semibold with bold large titles. One emphasis weight keeps the hierarchy in size and color instead of in heavier type. |
| Line height | About 1.25 to 1.5 times the size, in sp | Line heights in dp do not grow with the text. |
| Corner radius | 4, 8, 12, 16, 28 dp and full | Material's shape scale: fields 4, snackbars 4, chips 8, cards 12, large cards and buttons in square style 16, dialogs and sheet tops 28, pill buttons and switches full. Nested shapes stay concentric: inner radius = outer radius - gap. |
| Text contrast | At least 4.5:1 | 3:1 is enough only for large text: 24 sp regular or 19 sp bold (WCAG's 18 pt and 14 pt bold). Android's page reads the same thresholds as 18 and 14 sp; the WCAG sizes satisfy both. |
| Control edges and state markers | At least 3:1 | Field outlines, switch tracks, the fill or edge of the selected segment, against the adjacent colors. Disabled controls are exempt. |
| Text scale | Usable at 200 % | Android 14 and later; iOS's accessibility sizes go further. Large text grows less than small text on Android, so keep sizes and line heights in sp. |
| Line length | 45 to 75 characters | For paragraphs; on phones this usually holds by itself. |

## Applying them

- **Pick the app's own values once** from the ranges above and keep them as named tokens next to the shared components (spacing, radii, type styles, colors). Screens use the tokens, never ad hoc numbers.
- **Targets:** where the design calls for a small visual, such as a value chip or an icon, give it a hit area of 48 dp (`hitSlop` in React Native, `minimumInteractiveComponentSize` in Compose) and check that neighbours do not overlap.
- **Text scaling:** sizes in sp, line heights in sp or as a multiple, no fixed heights on anything that holds text, and rows that wrap: a label and its value side by side stack when the text no longer fits. Long names wrap to two lines or truncate with the full text reachable.
- **Theme colors:** check every foreground against every background it is drawn on, in each theme, and fix the theme rather than the screen. Translucent colors are measured after blending.
- **Selected states** are distinct by more than color alone: fill plus a check, an edge, or a weight change.
- **Changing numbers** use tabular figures so digits do not shift as they change.
- **Deviations:** when the platform, the library, or the owner's choice calls for another value, record it in the project's UI guide with the reason. Do not mix two values for the same role in one app.

## Sources

| Source | What it gives | Reviewed |
| --- | --- | --- |
| [Android: Core app quality guidelines](https://developer.android.com/docs/quality-guidelines/core-app-quality) | 48 dp targets, contrast, content descriptions, Back, state, permissions, themes | 2026-10-27 |
| [Android: Make apps more accessible](https://developer.android.com/guide/topics/ui/accessibility/apps) | Contrast thresholds, 48 dp targets, content descriptions | 2026-10-27 |
| [Android 14: Non-linear font scaling to 200 %](https://developer.android.com/about/versions/14/features#non-linear-font-scaling) | sp units, line heights in sp, testing at 200 % | 2026-10-27 |
| [Android: Window size classes](https://developer.android.com/develop/ui/compose/layouts/adaptive/window-size-classes) | Width breakpoints at 600, 840, 1200, and 1600 dp | 2026-10-27 |
| [Material 3: Breakpoints](https://m3.material.io/foundations/layout/breakpoints/medium) | 16 dp compact and 24 dp medium margins | 2026-10-27 |
| [Compose Material 3 tokens](https://github.com/androidx/androidx/tree/androidx-main/compose/material3/material3/src/commonMain/kotlin/androidx/compose/material3/tokens) | Button, field, list, navigation bar, slider, switch, dialog and sheet sizes, the shape scale, and the type scale (tokens v0_11_0) | 2026-10-27 |
| [Apple: Accessibility](https://developer.apple.com/design/human-interface-guidelines/accessibility) | 44 × 44 pt controls, spacing around controls, text contrast | 2026-10-27 |
| [Apple: Typography](https://developer.apple.com/design/human-interface-guidelines/typography) | 17 pt body, 11 pt minimum, Dynamic Type sizes | 2026-10-27 |
| [WCAG 2.2: Contrast (minimum)](https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html) | 4.5:1 text, 3:1 large text | 2026-10-27 |
| [WCAG 2.2: Non-text contrast](https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast.html) | 3:1 for controls and state indicators | 2026-10-27 |
| [WCAG 2.2: Dragging movements](https://www.w3.org/WAI/WCAG22/Understanding/dragging-movements.html) | An alternative to dragging, such as typing a slider's value | 2026-10-27 |
| [WCAG 2.2: Target size (enhanced)](https://www.w3.org/WAI/WCAG22/Understanding/target-size-enhanced.html) | 44 × 44 px targets for touch | 2026-10-27 |
