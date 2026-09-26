---
name: mobile-ui-basics
description: "Build, change, or review mobile screens for usable text entry, layouts, touch controls, feedback, and navigation. Apply to native apps and supported mobile web views; unrelated backend work or desktop-only UI does not trigger it."
---

# Mobile UI Basics

Keep mobile screens usable throughout an interaction, including while typing, scrolling, loading, opening an overlay, and going back. Preserve the project's visual design, platform conventions, and existing components; this skill does not prescribe a new design system or library.

## Scope the work

Apply the relevant checks to the changed screen, shared container, or reported defect. Adding an input should include its keyboard behavior; adjusting a button's hit area does not require reviewing every form. For a review or explanation, report findings without implementing changes or installing dependencies unless requested. Installation alone does not authorize fixing the whole app.

Identify the supported platforms, navigation and overlay libraries, shared input/layout components, and applicable project rules. Reuse established solutions and recorded exceptions. Respect explicit project exceptions and resolved user choices. If an unresolved project requirement conflicts with this guidance, identify the difference and ask whether to adjust it or record a scoped exception before the disputed change.

## Text entry and keyboard

- Keep the focused editable control and insertion point visible above the software keyboard. Include multiline editors, reply composers, search, and inputs inside dialogs or sheets. If an editor is taller than the available space, keep its active line visible and the rest reachable by scrolling.
- Handle both making room for the keyboard and revealing the focused field. A container that shrinks correctly can still leave the last field hidden. Switching fields while the keyboard is already open must work too.
- Reuse a shared solution at the appropriate screen or overlay boundary. Coordinate keyboard avoidance, scrolling, safe areas, and fixed controls so the same obstruction is not compensated twice. Avoid hardcoded keyboard heights and per-field delay patches.
- Preserve entered text, selection, and focus during layout changes. Keep Next, Done, submission, and dismissal consistent with the control's purpose; multiline entry should not accidentally submit. Do not steal focus back after the user dismisses the keyboard or leaves the screen.

For keyboard defects or changes to editable layouts, read [keyboard handling](references/keyboard-handling.md), using the section for the actual runtime. It covers React Native/Expo and mobile web without requiring either stack.

## Layout, scroll, and touch

- Account for the actual visible screen, safe areas, navigation bars, and overlays. Keep essential content and actions reachable on the supported small screens and with enlarged text. Avoid fixed screen heights that clip content; permit scrolling when content no longer fits.
- Give each scrollable region clear ownership and bounded space. Check the end of the content as well as the initial view: no hidden last row, unreachable action, or leftover keyboard-sized blank area. Nested scrolling should serve an intentional interaction.
- Size and space touch targets according to the project's platform guidance. A small icon can have a larger hit area, but adjacent targets must not overlap. Check nested pressables and scroll containers for swallowed or accidental taps; an enabled action should respond to the first intentional tap.
- Use meaningful labels, roles, and selected/disabled states, including for icon-only controls. Preserve readable text and control contrast, visible focus for external-keyboard users, text scaling, and supported assistive navigation. Communicate essential state beyond color alone.

## Feedback and navigation

- Distinguish loading, empty, error, and success when the affected flow can produce them. Provide useful recovery from errors and retain user input on a failed submission. Prevent duplicate submissions while work is pending and make that state apparent.
- Associate validation feedback with the relevant field and reveal it when necessary. Use the platform's accessible announcement mechanism where appropriate without announcing every update or covering the active control.
- Keep Back, close, gestures, and cancellation consistent with the navigation stack and platform. Transient state such as a keyboard, dialog, selection, or local zoom may consume Back before leaving a screen when that matches the interaction. Do not hardcode one universal priority or trap the user in an overlay.
- Confine focus and interaction to a modal while it is active; hidden screens must not intercept controls. Restore focus to the invoking control or another sensible destination when closing. Preserve drafts and reading position where the flow calls for them.
- Use motion to explain transitions without blocking input. Respect reduced-motion settings and handle repeated taps or interrupted transitions without duplicate routes or stale updates.

## Transient notifications

- Show brief in-app notifications, such as confirmations and informational toasts, as floating overlays. They must not reserve a row, push content, resize the screen, or change its scroll position when appearing or disappearing. This rule concerns transient UI feedback, not a notification inbox or operating-system push notifications.
- Dismiss them automatically after a few seconds. Reuse the project's duration or choose a short, readable default; accommodate accessibility timing needs and allow enough time for any interaction. Cancel obsolete timers so an earlier notification cannot dismiss its replacement.
- Respect safe areas, the keyboard, and the active overlay. Keep the focused field and essential controls unobstructed; transparent overlay space must not intercept taps. Announce relevant feedback accessibly without taking focus from the user's task.
- Do not make a disappearing toast the only source of a blocking error or required action. Keep that information available in the relevant field, screen, or dialog; its supplementary notification can still float and disappear.

## Verify the affected behavior

Choose cases relevant to the change; this is not a mandatory full-app audit:

| Changed area | Useful observations |
| --- | --- |
| Editable layout | Focus the last field, switch fields with the keyboard open, type multiple lines, submit or dismiss, and reopen. Check the applicable modal/sheet as well as normal screens. |
| Layout or scrolling | Use a supported small viewport and enlarged text; reach the last content/action and return without clipping or excess blank space. |
| Controls or requests | Activate once, try repeated input while pending, and exercise the relevant empty/error/retry path without losing the draft. |
| Transient notifications | Show, replace, and let a notification expire: content and scroll position stay stable, underlying controls remain usable, and an old timer cannot hide a newer message. Check placement with the keyboard open when relevant. |
| Navigation or overlays | Go back through the affected states, dismiss an overlay, and confirm that underlying screens cannot consume input while hidden. |
| Accessibility or motion | Check relevant labels, focus order, state announcements, and reduced motion using the supported platform tools. |

Use existing checks and focused interaction tests where they provide meaningful coverage. A component's presence or a mocked keyboard event alone does not prove visibility. Mobile browser emulation does not establish native keyboard behavior: use a supported device or simulator with its software keyboard for that claim, and distinguish it from browser layout checks.

Report the behavior changed, the platforms and cases actually checked, and any remaining device validation. Keep documentation proportional to durable behavior changes; do not create an audit report merely because this skill ran. Complete authorized work even when a device is unavailable, while leaving that verification explicitly pending.
