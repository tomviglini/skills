---
name: desktop-ui-design
description: "Build, change, or review desktop application screens: the hand pointer on everything clickable, layouts that stay put while content changes, consistent proportions (spacing, control heights, alignment, type, corners, contrast, target size), short interface text, and verification by looking at every changed screen in each theme and state. Apply to the windows a desktop app draws itself, including web UIs shown in a desktop window or browser; mobile screens, backend work, and the operating system's own dialogs and menus do not trigger it."
---

# Desktop UI Design

Keep a desktop app's own screens predictable under the mouse, steady while their content changes, consistent in their proportions, and readable at a glance. The proportions below are reference values drawn from the platform's guidelines and widely used design systems. A project may choose other values; it records them, with the reason, in its UI guide, and those recorded values win. Preserve the project's components and the choices it has already recorded.

## Scope the work

Apply the rules to the screens, components, or texts a change touches. Adding a button includes its pointer, its height, its place in the row, and its text; it does not require reviewing every window. For a review or explanation, report findings without implementing changes unless requested.

Respect explicit project exceptions and resolved user choices. If an unresolved project requirement conflicts with this guidance, identify the difference and ask whether to adjust it or record a scoped exception before the disputed change. To let the owner choose values interactively, such as a spacing or a color, use a tuning page (design-tuning-lab) rather than guessing.

## Pointer

- Everything clickable that the app draws itself shows the hand pointer: buttons, switches, checkboxes with their labels, tabs, segmented options, menu triggers, tiles, links, and other custom controls, whether drawn natively or styled in a web UI.
- Disabled controls and informative elements that do nothing when clicked, such as status text or measurement columns, keep the arrow. Text fields keep the text cursor.
- Component libraries often keep the arrow on their buttons and tabs. Build the hand into the project's own constructors or base styles, so every new control gets it and a disabled one returns to the arrow.
- The operating system's own controls, such as its dropdown menus, message boxes, dialogs, title bar, and edit controls, keep the system's pointers. Do not subclass or restyle them only to force a hand.

## Layout that stays put

- Changing content must not move the controls around it. A status that alternates between short and long texts, a label that becomes "Checking…", a count, or a progress state keeps buttons and fields where they were. A control moving under the pointer causes clicks on the wrong thing.
- Fix the arrangement instead of letting it depend on how much text fits: put a button in one place for every state, give a button whose label changes the width of its longest label, give changing values a fixed width, and place lines whose number varies below the controls, not above them.
- Lay out for short, typical, and very long content: names and paths truncate with an ellipsis inside their column, and the full text stays reachable where it matters, such as in a tooltip.
- Transient messages float over the content, away from the main actions, instead of inserting a row that pushes it.

## Proportions

Read [proportions](references/proportions.md) for the reference values, their sources, and how to adapt them to a project's platform or component library. In short:

- **Spacing:** one scale in steps of 4 px. Closer means related: 4 to 8 inside a control, 8 between controls in a row, 12 between a label and its control and between rows, 16 inside a card's edge, 16 to 24 between cards or sections.
- **Control height:** one height for every control that shares a row, such as buttons, fields, segmented controls, and path boxes (32 to 36 px at standard density). Smaller sizes are for icon buttons inside dense lists only.
- **Alignment:** labels share one fixed-width column across sections, so the controls start at the same edge. Help text and secondary buttons that belong to a control sit in the controls' column. Text is left-aligned.
- **Type:** one small ramp, such as 12 for captions and secondary lines, 14 for body text, 14 semibold for titles inside a page, and 18 to 20 for window and dialog titles. Regular and semibold weights only, sentence case, nothing below 12.
- **Corners:** 4 to 8 px on controls and 8 to 12 on cards and dialogs. A shape inside another keeps the curve concentric: its radius is the outer radius minus the gap between them.
- **Contrast:** text at least 4.5:1 against its background. Control edges, and whatever marks the picked or checked state, at least 3:1 against the colors next to them, in every theme. Hover and disabled states are exempt.
- **Targets:** every clickable element is at least 24 × 24 px; a checkbox or radio button shares one target with its label.
- **States:** hover, pressed, and focus are more visible than the resting state. In a group of options, hover changes only the background, so only the picked option reads as picked.

## Interface text

- Each text says what happened or what the user can do, in as few words as carry it.
- Leave out how the app works inside, what another element on the screen already says, and reassurances about behavior users already expect. Where something unexpected does happen, the screen that handles it says so.
- One caption per control: a section description does not repeat the caption of the control it contains.

## Verify the affected screens

Read [verification](references/verification.md) for capturing screens without disturbing the user's desktop, checking the pointer automatically, and measuring contrast.

| Changed area | Useful observations |
| --- | --- |
| Every changed screen | Look at it, in every theme the app offers and at the narrowest supported window size, through screenshots rather than the code alone. |
| Clickable elements | The hand over each one, the arrow over the informative parts and disabled controls next to them. System menus and dialogs keep their own pointers. |
| Changing content | Every state the content can take (idle, working, success, each error), with very long names and texts: no control moves between states, and nothing overflows its column. |
| Proportions | Controls in a row share one height and line up; labels and controls keep their columns; new colors meet the contrast minimums in each theme. |
| Text | Each new or changed text, read in place: it states the outcome or the action, and nothing on the same screen repeats it. |

When the owner reviews a visual change, show the screens before and after, side by side. Report which screens, themes, and states were checked and what could not be, such as hovering on the user's real desktop. Keep documentation proportional to durable behavior changes, and record the project's chosen values in its UI guide.
