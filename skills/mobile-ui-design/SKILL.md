---
name: mobile-ui-design
description: "Build, change, or review mobile screens: the platform's conventions (edge-to-edge and insets, system Back, permissions), consistent proportions (spacing, touch targets, control heights, type, corners, contrast, text scaling), where to edit, remove, and confirm things, usable text entry above the keyboard, layouts and feedback that stay put, short interface text, and verification by looking at every changed screen on an emulator or device. Apply to native Android and iOS apps and to web UIs shown on a phone; desktop windows and backend work do not trigger it."
---

# Mobile UI Design

Keep a mobile app's screens at home on the platform, consistent in their proportions, usable with one thumb, a large text size, and the keyboard open, and steady while their content changes. The values below are references drawn from Android's and Apple's guidelines, Material 3, and WCAG. A project may choose other values; it records them, with the reason, in its UI guide, and those recorded values win. Preserve the project's components, libraries, and the choices it has already recorded.

## Scope the work

Apply the rules to the screens, components, or texts a change touches. Adding an input includes its label, its keyboard behavior, and its place in the form; adjusting a button's hit area does not require reviewing every screen. For a review or explanation, report findings without implementing changes or installing dependencies unless requested.

Identify the supported platforms and their target SDK or OS versions, the navigation and overlay libraries, the shared components, and the project's recorded rules. Reuse established solutions and recorded exceptions. If an unresolved project requirement conflicts with this guidance, identify the difference and ask whether to adjust it or record a scoped exception before the disputed change. To let the owner choose values interactively, such as a spacing or a color, use a tuning page (design-tuning-lab) rather than guessing.

## Platform

Read [Android](references/android.md) when changing an Android app's window, navigation, permissions, notifications, or target SDK. In short:

- **Edge to edge:** the app draws behind the status and navigation bars; backgrounds and scrolling content extend under them, while text and controls are padded by the system bar, display cutout, and keyboard insets. The last item of a list scrolls clear of the navigation bar.
- **System Back** works everywhere: it closes the innermost transient layer first (keyboard, menu, dialog, sheet, an editor with unsaved changes asking first), then the screen, then returns to the start tab, then leaves the app. Do not replace it with on-screen back prompts or intercept it to log. Where the framework does not support predictive Back yet, keep the platform's opt-out and record why.
- **Permissions:** ask for the minimum, at the moment the user starts the feature that needs it, never at launch; explain why when the platform says a rationale is due; keep working without it and do not nag. Apps that talk to devices on the local network need Android's local network permission once they target the SDK that enforces it.
- **Window sizes:** layouts work in portrait and landscape, split screen, foldables, and tablets; large screens ignore orientation locks on recent Android versions. Choose the layout by the window's width class, not by device type, and keep state across rotation and folding.

On iOS, follow Apple's Human Interface Guidelines for the same topics: safe areas, swipe to go back, sheets dismissed by swiping down, and permission prompts with a purpose string.

## Proportions

Read [proportions](references/proportions.md) for the reference values, their sources, and how to adapt them. In short:

- **Spacing:** one scale in steps of 4 dp. Page margins 16 dp on phones and 24 dp from 600 dp wide; 8 dp between adjacent targets; 12 between rows; 16 inside a card's edge; 16 to 24 between cards or sections.
- **Touch targets:** at least 48 × 48 dp on Android and 44 × 44 pt on iOS, including icon buttons and the value chips of sliders. A smaller visual can have a larger hit area, but neighbouring hit areas never overlap.
- **Control height:** one height for every control that shares a row. Buttons 40 to 48 dp, text fields 48 to 56 dp, list rows 56 dp for one line and 72 for two.
- **Type:** text sizes in sp (Dynamic Type styles on iOS) so the user's text size applies. Body 16 for reading text and inputs, 14 for secondary text and button labels, 12 for captions and never smaller, 16 to 20 semibold for card titles, 22 to 28 for screen titles. Regular and semibold; bold only for the screen's title.
- **Text size:** every screen stays usable at 200 % text: rows wrap or stack instead of clipping, and nothing that holds text has a fixed height.
- **Corners:** the platform's shape scale, such as Material's 4, 8, 12, 16, and 28 dp plus full pills: 12 to 16 on cards, 28 on dialogs and the top of sheets, pills or 12 on buttons. Nested shapes stay concentric.
- **Contrast:** text at least 4.5:1 against its background; control edges and whatever marks the selected or checked state at least 3:1 against the colors next to them, in every theme the app offers.
- **States:** pressed and focused states are visible; disabled controls look disabled and say why nearby when the reason is not obvious.

## Components and patterns

Read [patterns](references/patterns.md) for editing, removing, confirming, sheets and dialogs, and lists of items. In short:

- **Control where it is used, manage elsewhere.** A card or row carries the everyday controls of its item. Renaming, connection details, and removing open a dedicated editor from one labeled Edit action or a settings icon, instead of turning the card's title into a field or keeping rare actions on every card.
- **Editors:** on a phone, a full-screen dialog with Close, a title, and Save in its top bar, so Save stays visible above the keyboard; from 600 dp wide, a dialog. Fields have persistent labels, start filled with the current values, and show errors under the field. Leaving with unsaved changes asks to keep editing or discard.
- **Dialogs and sheets:** a dialog for a short decision, a bottom sheet for a choice or a short task without typing, a full-screen dialog for a form. One at a time, each with an obvious way to close, and Back closes it.
- **Destructive actions** sit away from frequent controls, at the end of the item's editor, in the danger color. Prefer undo for what can be restored; confirm what cannot, with buttons named by their outcome ("Remove", "Keep").
- **Values set by dragging,** such as sliders, also accept a tap or a typed value.

## Text entry and keyboard

- Keep the focused editable control and insertion point visible above the software keyboard. Include multiline editors, reply composers, search, and inputs inside dialogs or sheets. If an editor is taller than the available space, keep its active line visible and the rest reachable by scrolling.
- When a field has an associated primary action, such as Send, Search, or Continue, reveal the field and action together when they fit. Keep the same keyboard clearance below the action that would otherwise be below the field. If the group cannot fit, prioritize the active text and keep the action reachable by scrolling; do not jump to a distant form submit button or hide the focused field to expose it. An action in a top bar that stays visible avoids the problem.
- Handle both making room for the keyboard and revealing the focused field. A container that shrinks correctly can still leave the last field hidden. Switching fields while the keyboard is already open must work too.
- Reuse a shared solution at the appropriate screen or overlay boundary. Coordinate keyboard avoidance, scrolling, safe areas, and fixed controls so the same obstruction is not compensated twice. Avoid hardcoded keyboard heights and per-field delay patches.
- Preserve entered text, selection, and focus during layout changes. Keep Next, Done, submission, and dismissal consistent with the control's purpose; multiline entry should not accidentally submit. Use the keyboard type that fits the value (numbers, decimal, email, URL). Do not steal focus back after the user dismisses the keyboard or leaves the screen.

For keyboard defects or changes to editable layouts, read [keyboard handling](references/keyboard-handling.md), using the section for the actual runtime. It covers React Native/Expo and mobile web without requiring either stack.

## Layout, scroll, and touch

- Account for the actual visible screen, insets, navigation bars, and overlays. Keep essential content and actions reachable on the smallest supported screen and with enlarged text. Avoid fixed screen heights that clip content; permit scrolling when content no longer fits.
- Changing content must not move the controls around it: a status that alternates between texts, a progress state, or a count keeps buttons and sliders where they were. Reserve the space of a spinner or a changing label.
- Give each scrollable region clear ownership and bounded space. Check the end of the content as well as the initial view: no hidden last row, unreachable action, or leftover keyboard-sized blank area. Nested scrolling should serve an intentional interaction.
- Check nested pressables and scroll containers for swallowed or accidental taps; an enabled action responds to the first intentional tap. Keep frequent actions within reach of the thumb, lower on the screen, and rare or destructive ones away from them.
- Use meaningful labels, roles, and selected or disabled states, including for icon-only controls. Keep visible focus for external-keyboard users and support the platform's screen reader. Communicate essential state beyond color alone.

## Feedback and navigation

- Distinguish loading, empty, error, and success when the affected flow can produce them. Provide useful recovery from errors and retain user input on a failed submission. Prevent duplicate submissions while work is pending and make that state apparent.
- Associate validation feedback with the relevant field and reveal it when necessary. Use the platform's accessible announcement mechanism where appropriate without announcing every update or covering the active control.
- Keep Back, close, gestures, and cancellation consistent with the navigation stack and platform, as described under Platform. Do not trap the user in an overlay.
- Confine focus and interaction to a modal while it is active; hidden screens must not intercept controls. Restore focus to the invoking control or another sensible destination when closing. Preserve drafts and reading position where the flow calls for them.
- Use motion to explain transitions without blocking input. Respect reduced-motion settings and handle repeated taps or interrupted transitions without duplicate routes or stale updates. Haptics follow the system setting and mark discrete events, such as a slider's steps, not every frame.

## Interface text

- Each text says what happened or what the user can do, in as few words as carry it.
- Leave out how the app works inside, what another element on the screen already says, and reassurances about behavior users already expect. Where something unexpected does happen, the screen that handles it says so.
- One caption per control: a section description does not repeat the caption of the control it contains.

## Transient notifications

- Show brief in-app notifications, such as confirmations and informational toasts, as floating overlays. They must not reserve a row, push content, resize the screen, or change its scroll position when appearing or disappearing. This rule concerns transient UI feedback, not a notification inbox or operating-system notifications.
- Dismiss them automatically after a few seconds, longer when they offer an action such as Undo. Reuse the project's duration or choose a short, readable default; accommodate accessibility timing needs. Cancel obsolete timers so an earlier notification cannot dismiss its replacement.
- Respect insets, the keyboard, and the active overlay. Keep the focused field and essential controls unobstructed; transparent overlay space must not intercept taps. Announce relevant feedback accessibly without taking focus from the user's task.
- Do not make a disappearing toast the only source of a blocking error or required action. Keep that information available in the relevant field, screen, or dialog; its supplementary notification can still float and disappear.

## Verify the affected screens

Read [verification](references/verification.md) for capturing screens on an emulator without disturbing the user's desktop, changing text size, theme, navigation mode, and window size from the command line, checking target sizes and labels automatically, and using a simulated device instead of real hardware.

| Changed area | Useful observations |
| --- | --- |
| Every changed screen | Look at it through screenshots, in every theme the app offers, at the smallest supported width, and at 200 % text. |
| Window and navigation | Gesture and three-button navigation; landscape and a 600 dp or wider window where supported; Back through every affected state. |
| Editable layout | Focus the last field, switch fields with the keyboard open, type multiple lines, submit or dismiss, and reopen, in dialogs, sheets, and normal screens. |
| Changing content and requests | Every state the content can take (loading, empty, each error, success), with very long names: no control moves between states. Activate once, try repeated input while pending, and retry without losing the draft. |
| Proportions and accessibility | Targets of at least 48 dp, labels on icon-only controls, contrast of new colors in each theme, focus order and announcements with the screen reader where they changed. |
| Transient notifications | Show, replace, and let one expire: content stays still, controls underneath stay usable, an old timer cannot hide a newer message. |

A component's presence or a mocked keyboard event alone does not prove visibility. Mobile browser emulation does not establish native keyboard behavior: use an emulator, simulator, or device with its software keyboard for that claim, and distinguish it from browser layout checks.

When the owner reviews a visual change, show the screens before and after, side by side. Report which screens, states, and settings were checked and what was not, such as real Wi-Fi discovery or touch on a physical phone. Keep documentation proportional to durable behavior changes, and record the project's chosen values in its UI guide.
