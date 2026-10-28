# Proportions

Read this reference when choosing or reviewing spacing, control sizes, alignment, type, corners, colors, or target sizes on a desktop screen. The values are references, not a design system: they keep a project's screens consistent with each other and with the platform. A project that needs other values records them, with the reason, in its UI guide; the recorded values then apply to that project.

Sources were reviewed on 2026-10-26. They are listed at the end; the values are summarized here, not copied.

## Where the values come from

- **The platform** sets the numbers a desktop app should feel at home with. On Windows these are Microsoft's Windows 11 and Fluent 2 guidelines: a 4 px spacing scale, 32 px standard controls, a type ramp built around a 14 px body, and 4 px corners on controls with 8 px on overlays. A macOS app follows Apple's guidelines instead.
- **The component library** sets the proportions of its own components. Many current libraries, including GPUI's gpui-component, follow shadcn/ui: 36 px buttons, fields, and tab lists (32 px small), 8 to 10 px corners, and 24 px card padding. Keep the library's proportions where they already agree with the rules below rather than restyling every component.
- **Accessibility** sets the minimums that do not move: WCAG 2.2 contrast for text and for controls and their states, and the minimum target size.
- **Practitioners' checklists**, such as Vercel's Web Interface Guidelines and the book Refactoring UI, cover the polish that the numbers alone miss: long content, alignment, hover contrast, and spacing systems.

## Reference values

| Topic | Reference | Notes |
| --- | --- | --- |
| Spacing scale | 4, 8, 12, 16, 24, 32 px | Steps of 4. Values like 2, 3, or 5 appear only as optical corrections, such as aligning an icon. |
| Inside a control | 4 to 8 px | For example, the inset of a segmented control's options or the gap between an icon and its label. |
| Between controls in a row | 8 px | Buttons, a field and its Browse button, a control and its action. |
| Label to control | 12 px | Labels in a column of fixed width, such as 120 px, shared by every section of the screen. |
| Between rows | 12 px | Rows of one card or group. |
| Card or surface edge to content | 16 px | 24 px in roomier layouts. |
| Between cards or sections | 16 to 24 px | More space between groups than inside them, so the grouping reads without dividers. |
| Control height | 32 to 36 px | One height per app for every control that shares a row. 24 to 28 px only for icon buttons in dense lists. |
| Body text | 14 px regular | The window's default size. |
| Captions and secondary lines | 12 px regular | Descriptions under a title, the second line of a list row, status bars. Never smaller. |
| Titles inside a page | 14 px semibold | Card and group titles. |
| Window and dialog titles | 18 to 20 px semibold | One per window or dialog. |
| Weights | Regular and semibold | No light, medium, or bold mixes. |
| Corner radius | 4 to 8 px controls, 8 to 12 px cards and dialogs | Nested shapes stay concentric: inner radius = outer radius - gap. |
| Text contrast | At least 4.5:1 | Every text color against every background it appears on, in each theme, including warning and success colors. |
| Control edges and state markers | At least 3:1 | Field borders, checkbox outlines, and the fill, edge, or mark that shows the picked option, against the adjacent colors. Hover and disabled states are exempt. |
| Target size | At least 24 × 24 px | Icon buttons in list rows included. A checkbox's label is part of its target. |

## Applying them

- **Pick the app's own values once** from the ranges above (for example, a 34 px control height and 7 px control corners) and keep them as named constants or tokens, next to the project's component constructors. Every screen uses the constants, never ad hoc numbers.
- **Theme colors:** check every foreground against every background it is drawn on, in each theme, and fix the theme rather than the individual screen. Colors that fail usually belong to a family (warning, success, borders), so the fix is a darker or lighter step of the same hue.
- **Segmented controls and tabs:** the picked option is distinct by more than color alone (fill plus an edge or a weight change), meets 3:1 against the track, and keeps its text weight steady between hover and rest. If the library draws the picked option in a color that matches the track in one theme, draw the control in the project instead of fighting the library's tokens.
- **Changing numbers** (times, percentages, counters) use tabular figures, so digits do not shift as they change. Many UI fonts, such as Segoe UI, already use tabular figures by default; check before adding a setting.
- **Deviations:** when the platform, the library, or the owner's choice calls for another value, record it in the project's UI guide with the reason. Do not mix two values for the same role in one app.

## Sources

| Source | What it gives | Reviewed |
| --- | --- | --- |
| [Microsoft: Content layout and spacing](https://learn.microsoft.com/en-us/windows/apps/design/basics/content-basics) | Spacing between buttons, labels, content areas, and surface edges | 2026-10-26 |
| [Microsoft: Geometry in Windows 11](https://learn.microsoft.com/en-us/windows/apps/design/signature-experiences/geometry) | Corner radii for controls, overlays, and windows | 2026-10-26 |
| [Microsoft: Typography in Windows](https://learn.microsoft.com/en-us/windows/apps/design/signature-experiences/typography) | Type ramp, weights, minimum sizes, sentence case, truncation | 2026-10-26 |
| [Microsoft: Spacing and density](https://learn.microsoft.com/en-us/windows/apps/design/style/spacing) | Standard and compact sizing targets | 2026-10-26 |
| [Fluent 2: Layout](https://fluent2.microsoft.design/layout) | The 4 px base unit and spacing ramp, proximity, alignment | 2026-10-26 |
| [shadcn/ui components](https://github.com/shadcn-ui/ui/tree/main/apps/v4/registry/new-york-v4/ui) | Button, input, tab list, and card sizes in the `new-york-v4` registry | 2026-10-26 |
| [WCAG 2.2: Non-text contrast](https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast.html) | 3:1 for controls and state indicators | 2026-10-26 |
| [WCAG 2.2: Target size (minimum)](https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html) | 24 × 24 px targets | 2026-10-26 |
| [web.dev: Cumulative Layout Shift](https://web.dev/articles/cls) | Why unexpected shifts cause wrong clicks | 2026-10-26 |
| [Nielsen Norman Group: Proximity](https://www.nngroup.com/articles/gestalt-proximity/) | Spacing within and between groups | 2026-10-26 |
| [Atlassian: Spacing](https://atlassian.design/foundations/spacing) | Small, medium, and large spacing ranges | 2026-10-26 |
| [Vercel: Web Interface Guidelines](https://vercel.com/design/guidelines) | Long content, hit targets, tabular numbers, hover contrast | 2026-10-26 |
