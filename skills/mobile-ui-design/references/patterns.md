# Patterns

Read this reference when deciding where an item is edited or removed, when adding a dialog, sheet, or editor, when a screen lists items that can be controlled and managed, or when an action needs confirmation. Reviewed on 2026-10-27; the sources are listed at the end.

## Control and manage in different places

An item that the user controls often and manages rarely, such as a light, a speaker, or a saved preset, separates the two:

- **The card or row is for control.** It shows the item's name and state and the controls used every day: power, a level, a mode. Its layout stays the same whether the item is busy, offline, or being changed.
- **Management opens an editor.** Renaming, connection details, defaults, and removing live in the item's editor, reached from one labeled Edit action on the card or a settings icon in its header. Smart-home apps from the platform makers work this way: the tile controls the device, and the device's settings hold its name, its details, and Remove at the bottom.
- **Avoid turning part of the card into a field.** An inline rename makes the card change shape, puts the field and its Save apart when the card is tall, hides how to leave it, and competes with the keyboard for the space above. Inline editing fits a single value whose field and confirmation sit together, such as typing a slider's exact value, and lists made for editing, such as a to-do list.
- **Rare actions do not repeat on every card.** Remove and other rare or destructive actions belong in the editor or an overflow menu, not as permanent buttons next to everyday controls.

## Editors

- **Phone width:** a full-screen dialog: a top bar with Close (×) at the start, the task as its title ("Edit light"), and Save at the end. Save stays visible above the keyboard, so the form needs no extra clearance to reveal it. From 600 dp wide, the same content in a dialog of about 560 dp with Cancel and Save at its bottom.
- **Fields:** each with a persistent label above or inside it (not only a placeholder), filled with the current values, the right keyboard type, and the first field focused only when the editor exists to change it. Read-only details, such as a model or an address the user cannot change, appear as text below the fields.
- **Save:** enabled once a valid change exists. Validate on Save and show each error under its field, keeping the editor open and the input intact. Close after saving and confirm in a brief notification only when the change is not visible on the screen underneath.
- **Leaving:** Close and Back with unsaved changes ask "Discard changes?" with Keep editing and Discard; without changes they simply close.
- **Item actions** that are not fields, such as saving the current state as the default, go in their own section below the fields; Remove goes last, set apart, in the danger color.

## Dialogs, sheets, and full screens

| Use | When |
| --- | --- |
| Dialog | A short decision or a single piece of information: confirm a removal, choose between two outcomes, a short text such as a name when there is no editor. |
| Bottom sheet | A choice or a short task without typing, close to where it started: pick a scene, share, a list of actions for an item. |
| Full-screen dialog | A form, a task with several fields, or anything that needs the keyboard on a phone. |
| New screen | Content the user reads or browses and returns from with Back. |

- One dialog or sheet at a time; close the first before opening another. A full-screen dialog is the only one another dialog, such as a discard confirmation, may appear over.
- Each one names its task in its title, has a visible way to close, closes with Back, and, for sheets, with a swipe down or a tap on the scrim. Closing never loses input without asking.

## Destructive actions

- **Undo when it can be restored:** perform the action and offer Undo in a notification that stays a few seconds longer than usual.
- **Confirm when it cannot:** a dialog that says what will be lost, with buttons named by their outcome ("Remove", "Cancel"), the destructive one in the danger color and not the default. Rare and severe actions only; confirmations shown often get dismissed without reading.
- Keep destructive actions away from frequent controls and out of the reach of an accidental tap: at the end of an editor or in a menu.

## Lists of items

- **Empty:** the first screen of a new install says what is missing and offers the action that fills it ("Discover lights", "Add by IP address").
- **Order and identity:** items keep their place while their state changes; an item that is busy or offline shows it in place instead of moving or disappearing.
- **Long names** wrap to two lines or truncate with the full name in the editor.
- **Group actions** that affect every item ("All off") sit above the list; setup actions that are used once sit below it or in the empty state.

## Values and controls

- **Sliders:** a slider for coarse changes, plus its value as a tappable chip that accepts a typed number for exact ones, which also serves users who cannot drag. Commit on release, not on every frame.
- **Switches** apply at once; a switch inside an editor that has Save applies on Save, and then a checkbox fits better.
- **Segmented buttons** for two to five exclusive options; the selected one marked by fill and a check or weight, not color alone.

## Sources

| Source | What it gives | Reviewed |
| --- | --- | --- |
| [Material 3: Dialogs](https://m3.material.io/components/dialogs/guidelines) and [Material Components for Android: Dialogs](https://github.com/material-components/material-components-android/blob/master/docs/components/Dialog.md) | Basic and full-screen dialogs, and when each fits | 2026-10-27 |
| [Material Components for Android: Bottom sheets](https://github.com/material-components/material-components-android/blob/master/docs/components/BottomSheet.md) | Standard and modal sheets | 2026-10-27 |
| [Apple: Modality](https://developer.apple.com/design/human-interface-guidelines/modality) | Short modal tasks, one at a time, confirming before losing input | 2026-10-27 |
| [Apple: Sheets](https://developer.apple.com/design/human-interface-guidelines/sheets) | Cancel and Done placement, swipe to dismiss, full-screen for complex tasks | 2026-10-27 |
| [Nielsen Norman Group: Confirmation dialogs](https://www.nngroup.com/articles/confirmation-dialog/) | Confirm only what cannot be undone, outcome-named buttons, prefer undo | 2026-10-27 |
| [WCAG 2.2: Dragging movements](https://www.w3.org/WAI/WCAG22/Understanding/dragging-movements.html) | A non-dragging alternative for sliders | 2026-10-27 |
