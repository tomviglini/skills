# Control Patterns

A compact pattern for a lab's controls, state, and compare behavior. It is plain browser JavaScript with no library; adapt names and structure to the page. Patterns reviewed on 2026-10-02 and exercised in Chromium; they are a sketch, not a package.

## Describe controls once

Keep one schema as the single source for labels, ranges, defaults, and the views each control affects. Derive the defaults from it so they cannot drift:

```js
const SCHEMA = [
  { key: 'size', label: 'Size', type: 'range', min: 28, max: 54, step: 0.5, unit: 'px', def: 38, views: ['desktop'] },
  { key: 'accent', label: 'Accent', type: 'color', presets: [['#7da36a', 'Green']], def: '#7da36a', views: ['*'], shared: true },
  { key: 'grid', label: 'Grid strength', type: 'range', min: 0, max: 1, step: 0.01, def: 0.3, views: ['*'], when: (view) => usesGrid(view) },
];
const DEF = Object.fromEntries(SCHEMA.map((c) => [c.key, c.def]));
let S = { ...DEF, view: 'desktop' };
```

`views` decides which tab shows a control, `when` hides it when it has no effect in the current state, and `shared` marks values that change every view. Build every control once and toggle visibility on view changes; rebuilding inputs while the user drags them interrupts the drag.

## Default, revert, and modified

Render the default as the control would name it, compare with a numeric tolerance, and disable revert and compare when nothing differs:

```js
const isDefault = (c) => typeof c.def === 'number' ? Math.abs(S[c.key] - c.def) < 1e-9 : String(S[c.key]).toLowerCase() === String(c.def).toLowerCase();
// On every sync: toggle a "modified" class, disable revert and compare when isDefault(c).
// Revert: S[c.key] = DEF[c.key]; save(); sync(); render();
```

Mark a slider's default position with an element placed at `(def - min) / (max - min)` of the track width, offset by half the thumb width.

## Press to compare

Swap the value only for the duration of one render, so saved state never changes:

```js
let peek = null; // a control key, or '*' for every default
function view() {
  if (!peek) return render();
  const saved = S;
  S = peek === '*' ? { ...S, ...DEF } : { ...S, [peek]: DEF[peek] };
  try { render(); } finally { S = saved; }
}
document.addEventListener('pointerdown', (e) => {
  const b = e.target.closest('[data-peek]');
  if (!b || b.disabled) return;
  e.preventDefault();
  b.setPointerCapture?.(e.pointerId);
  peek = b.dataset.peek; view();
});
['pointerup', 'pointercancel', 'lostpointercapture'].forEach((t) => document.addEventListener(t, () => { peek = null; view(); }));
```

Also support Space and Enter while the button has focus, release on window blur, and prevent the long-press context menu on touch screens with `touch-action: none`, `user-select: none`, and a `contextmenu` handler. Show a short banner naming what is being compared, for example "Showing the default of Size: 38 px".

## Persist and exchange settings

Wrap storage in `try`/`catch`; private windows and hosted previews can refuse it, and the page must still work. Copy settings with `navigator.clipboard.writeText` inside the click handler, falling back to an in-page text area when the clipboard is refused. Paste through an in-page text area with an Apply button, since hosted viewers often block `prompt`.

## Scale previews

Give each preview its native size, set `zoom` from the available width for a fit mode, and offer 100 % for true size. Recompute on resize, and only for visible views.
