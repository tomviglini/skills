---
name: design-tuning-lab
description: "Build or revise an interactive page where the owner tunes and compares visual design values (logo treatment, color, type, spacing) in real context, with visible defaults, revert, and compare controls. Implementing a chosen design in the product, static galleries, and ordinary UI fixes do not trigger this skill."
---

# Design Tuning Lab

A tuning lab is a page for deciding design values. The owner changes the values themselves, sees the result where it will actually live, and can always tell what they changed and how it differs from the proposal. The outcome is a set of values that the agent turns into a coherent system; the page is a decision tool, not the product.

These are working principles, not a fixed template. Adapt the controls, layout, and level of polish to the question being decided and to the project's existing tools and conventions.

Respect documented project exceptions and choices already resolved by the user. If this guidance conflicts with a project rule, such as its language or documentation policy, show the conflict and ask whether to adjust the rule or record a scoped exception before the disputed action.

## Show the real context

- Preview each value where it will be used: real captures, the product's own markup, or the running product itself, with its fonts, colors, and copy. A swatch or an isolated specimen alone does not show how a value works in place.
- State the scale of every preview and offer 1:1. When drawing over a capture, clear the original element and place the replacement from measured positions, such as baselines and boxes, rather than by eye.
- Offer a before/after toggle against what the product shows today, and keep the product's languages or themes switchable when they affect the result.

## One view at a time

- Give each place under review its own view or tab: a screen, a page section, a size. Show only the controls that change the visible view. When a group does not apply, say why instead of leaving inert controls, for example "this view has no grid".
- Mark controls whose value is shared across views, so a change made in one place is not mistaken for a local one. An "everything together" view lets the owner check the system as a whole.

## Make every change identifiable

Each control shows:

- Its current value, and its default written out, such as "Default: 38 px". On sliders, also mark the default position.
- A button that reverts that control to its default.
- A press-and-hold button that temporarily shows the default without changing the saved value.
- A "modified" marker while the value differs from the default.

At the page level, provide reset all, hold to see all defaults, and before/after. Treat the defaults as the agent's current recommendation, labeled separately from what the product uses today.

## Support iteration

- Keep the owner's settings in the browser with guarded storage, and let them copy the values as text to send back and paste values in. Presets can offer named starting points.
- Make the page self-contained, with fonts and images embedded or bundled, so it opens offline, can be hosted, and works at phone width. Avoid browser dialogs such as `prompt` and `alert`, which hosted viewers often block.
- Read [control patterns](references/control-patterns.md) when building the controls, state, and compare behavior from scratch.

## Interpret the values that come back

- Treat values the owner tuned by eye as a signal of intent, not a specification. Name the intent, for example a flatter, quieter grid, then fold it into a consistent system: shared tokens, ratios, and size rules that hold in every view.
- Present the adjusted values as the new defaults, explain what changed and why, and let the owner confirm them in the same page. Ask when the intent is ambiguous.
- Once a direction is chosen, record the decision and the page where the project keeps design evidence, following its documentation conventions. A lab value is not a production change until it is implemented and verified in the product.

## Verify

Open each view once, check for script errors and horizontal overflow at phone width, and try revert and compare on one control. Do not loop on re-verification; the owner reviews the live page.
