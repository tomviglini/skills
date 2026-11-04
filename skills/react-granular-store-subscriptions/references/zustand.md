# Zustand

## Example versions

| Scope | API target |
| --- | --- |
| React baseline | React 18.x function components, with TypeScript/TSX |
| Store and React adapter | `zustand` 5.x, using standard `create` / `useStore` APIs |
| Grouped selector results | `useShallow` from `zustand/react/shallow` |

Documentation reviewed: **2026-09-23**, using the official [v5 migration guide](https://github.com/pmndrs/zustand/blob/main/docs/reference/migrations/migrating-to-v5.md) and [useShallow reference](https://zustand.docs.pmnd.rs/reference/hooks/use-shallow).

Validation status: documentation-checked, illustrative snippets; no runtime test matrix or exact tested patch versions. These examples target v5 behavior. For v4, another major version, or `zustand/traditional`, check the installed API and use its matching subscription/equality pattern without requiring an upgrade.

## Store integration

Inspect the installed version and whether the application uses a bound hook, a vanilla store, or a scoped provider. Reuse that instance; do not create a store during each render or bypass a scoped provider with a global singleton.

## Select fields and actions independently

Assume `useCartStore` is the project's existing bound hook with `itemCount` and a stable `addItem` action:

```tsx
import { useCartStore } from './stores/cart';

export function CartCount() {
  const itemCount = useCartStore((state) => state.itemCount);
  return <span>{itemCount}</span>;
}

export function AddItemButton() {
  const addItem = useCartStore((state) => state.addItem);
  return <button onClick={() => addItem()}>Add item</button>;
}
```

Calling `useCartStore()` and then destructuring subscribes to the whole state. Also inspect convenience hooks that internally return the entire store. For a vanilla store, use the library's `useStore(store, selector)` integration with the correct store instance.

## Stable grouped results

For a small group of fields:

```tsx
import { useShallow } from 'zustand/react/shallow';

// Inside a component, using the existing bound hook:
const { itemCount, addItem } = useCartStore(
  useShallow((state) => ({
    itemCount: state.itemCount,
    addItem: state.addItem,
  })),
);
```

`useShallow` can reuse a shallow-equal result; it does not avoid running the calculation. Fresh nested objects still differ. Prefer primitive outputs or existing references for simple reads. See [preventing rerenders with useShallow](https://zustand.docs.pmnd.rs/learn/guides/prevent-rerenders-with-use-shallow.html).

## Version-sensitive pitfalls

- In Zustand v5's standard `create`/`useStore` APIs, selector outputs must be stable for unchanged state. Fresh arrays, objects, or fallback functions may cause snapshot warnings or update loops.
- Stabilize selector results with precise reads, `useShallow` where appropriate, or cached derivation. `useCallback` around a selector that still allocates a fresh result does not fix this.
- Do not pass a legacy equality-function argument to v5's standard bound hook. If the project already uses `createWithEqualityFn` or `useStoreWithEqualityFn` from `zustand/traditional`, follow that API and its peer dependency requirements.

See the official [Zustand v5 migration guide](https://github.com/pmndrs/zustand/blob/main/docs/reference/migrations/migrating-to-v5.md).

Use stable defaults rather than allocating within selectors:

```tsx
const EMPTY_IDS: readonly string[] = [];

// Inside a component; never mutate EMPTY_IDS:
const ids = useCartStore((state) => state.visibleItemIds ?? EMPTY_IDS);
```

For nested derived collections, selecting a stable input and deriving during render may suffice. Cache expensive transformations deliberately. Avoid introducing deep comparison to conceal repeated allocations without measuring its cost.

`getState()` is suitable for an intentional imperative read in a handler, not for a displayed value that must update. Manual `subscribe` or `subscribeWithSelector` callbacks do not replace React's supported subscription hook. Preserve immutable references for unchanged branches, including stable action functions.
