---
name: desktop-ui-basics
description: "Build, change, or review desktop application screens for pointer feedback, layouts that stay put while their content changes, and short interface text. Apply to the windows a desktop app draws itself, including web UIs shown in a desktop window or browser; mobile screens, backend work, and the operating system's own dialogs and menus do not trigger it."
---

# Desktop UI Basics

Keep a desktop app's own screens predictable under the mouse and readable at a glance. Preserve the project's visual design, components, and platform conventions; this skill does not prescribe a design system or library.

## Scope the work

Apply the rules to the screens, components, or texts a change touches. Adding a button includes its pointer and its text; it does not require reviewing every window. For a review or explanation, report findings without implementing changes unless requested.

Respect explicit project exceptions and resolved user choices. If an unresolved project requirement conflicts with this guidance, identify the difference and ask whether to adjust it or record a scoped exception before the disputed change.

## Pointer

- Everything clickable that the app draws itself shows the hand pointer: buttons, switches, menu triggers, tiles, links, and other custom controls, whether drawn natively or styled in a web UI.
- Informative elements that do nothing when clicked, such as status text or measurement columns, keep the arrow. Text fields keep the text cursor.
- The operating system's own controls, such as its dropdown menus, message boxes, dialogs, title bar, and edit controls, keep the system's pointers. Do not subclass or restyle them only to force a hand.

## Layout that stays put

- Changing content must not move the controls around it. A status that alternates between short and long texts, a label that becomes "Checking…", a count, or a progress state keeps buttons and fields where they were.
- Fix the arrangement instead of letting it depend on how much text fits: put a button in one place for every state, reserve space for the longest expected label, or give changing values a fixed width.
- Transient messages float over the content instead of inserting a row that pushes it.

## Interface text

- Each text says what happened or what the user can do, in as few words as carry it.
- Leave out how the app works inside, what another element on the screen already says, and reassurances about behavior users already expect. Where something unexpected does happen, the screen that handles it says so.
- One caption per control: a section description does not repeat the caption of the control it contains.

## Verify the affected behavior

| Changed area | Useful observations |
| --- | --- |
| Clickable elements | Hover each one: the hand shows over it and the arrow over informative parts next to it. System menus and dialogs keep their own pointers. |
| Changing content | Step through every state the content can take (idle, working, success, each error) at the narrowest supported window: no control moves between states. |
| Text | Read each new or changed text in place: it states the outcome or the action, and nothing on the same screen repeats it. |

Report what was changed and which states and windows were checked. Keep documentation proportional to durable behavior changes.
