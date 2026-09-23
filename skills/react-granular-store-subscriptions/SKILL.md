---
name: react-granular-store-subscriptions
description: "Implement or refactor React store subscriptions so components observe only the state they need. Use when connecting components to Redux, Zustand, Jotai, MobX, Valtio, or similar stores, or diagnosing unnecessary store-driven rerenders and broad subscriptions."
---

# React Granular Store Subscriptions

Connect state at the smallest useful rendering boundary. Prefer precise subscriptions and stable selected values before adding `React.memo`, `useMemo`, or `useCallback`.

This reduces renders caused by unrelated store updates. It does not guarantee that a component never renders: parents, props, context, and local state can still cause renders. A selector running, a React render, a DOM commit, and a browser repaint are different events.

## Inspect the integration

- Identify the installed library and version, store definitions, provider scope, typed hooks, and existing selectors. Reuse the correct store instance for this screen, feature, or request.
- Check resolved versions in the relevant workspace's lockfile or installed packages, as well as declared ranges in `package.json`. Compare them with the reference's example targets. Adapt version-sensitive APIs to the project; do not upgrade dependencies merely to match a snippet. Example targets describe the code, while the subscription principles apply more broadly.
- Trace the state update to its subscribing components. Look for whole-store reads, broad object selections, subscriptions high in the tree, and custom hooks that hide broad subscriptions.
- Identify what each component actually displays and which actions it calls. Distinguish store-triggered renders from parent or context renders before changing code.
- Read only the relevant reference: [Redux](references/redux.md), [Zustand](references/zustand.md), or [other store models](references/other-stores.md). For an unfamiliar library, inspect its installed API and official documentation before choosing a subscription pattern.

## Shape subscriptions around consumers

1. Subscribe inside the component that renders the value. Extract a small subscriber when only one part of a large screen needs frequent updates. Keep ancestors free of subscriptions used only by descendants.
2. Select the exact primitive, entity field, atom, or tracked property needed. Destructuring after subscribing to an entire store or slice does not narrow that subscription. A whole entity is reasonable when the component actually renders its relevant fields together.
3. Give action-only controls the library's dispatch or write-only API, or select a stable action separately. Do not subscribe them to unrelated data merely to obtain a setter.
4. Prefer existing stable references or separate primitive selections. If a selector creates an object or array, use the library's supported equality or result-stabilization mechanism when justified. Stabilizing the selector function alone does not stabilize its return value.
5. For lists, prefer a parent subscribed to membership/order and children subscribed by stable item ID. Updating one item's field should not needlessly replace the ID collection or unrelated entities. Membership/order changes and parent renders can still render rows.
6. Keep updates compatible with the store's change detection. Preserve unchanged references in immutable stores; use the native observable or proxy mutation model where appropriate. Avoid deep cloning an entire store on a small update.

Do not replace the state library, introduce a second store, or globalize local state merely to apply this skill. Avoid mirroring selected values into component state through effects. Preserve store lifetimes and server request isolation.

## Use memoization deliberately

- First fix unnecessarily broad subscriptions, unstable selector outputs, and misplaced state ownership.
- Do not wrap every component or selector in memoization. Plain field lookups usually need none.
- `useMemo` applied after a broad subscription does not narrow that subscription. Select the required value directly instead of caching a lookup into a subscribed collection.
- Add `useCallback` only when function identity matters to a consumer or dependency. Ordinary event handlers do not need it by default.
- Keep or add selector-level caching when needed for expensive derived data or stable results. This differs from wrapping a component in `React.memo`.
- `useShallow`, computed values, and library internals may cache results; the aim is to avoid unnecessary manual component memoization, not to ban caching.
- Preserve memoization whose purpose still applies. If parent renders remain expensive after improving boundaries, measure and explain the targeted optimization instead of promising selectors solve them.
- Check whether React Compiler is enabled for the affected code. Its automatic memoization can reduce the need for manual wrappers; do not assume it is enabled from the React version alone or enable it just for this refactor. Precise store subscriptions still matter. See the [React Compiler introduction](https://react.dev/learn/react-compiler/introduction).
- Never suppress a legitimate update with an equality function that ignores rendered data or always returns `true`.

## Verify observable behavior

Use the project's existing checks and React Profiler or a focused render probe when available. For an implementation change, exercise these cases:

| Update | Expected observation |
| --- | --- |
| Unrelated store field | The target subscriber receives no render from that store change when its selected value is unchanged. |
| Selected field | The displayed value updates correctly. |
| Another entity's field | An unchanged entity subscriber receives no store-triggered render, provided its selection remains equal. |
| List insertion, removal, or reordering | Membership and order update; removed entities are handled safely. |
| Action invocation | The intended store changes and dependent views update. |
| Parent or context update | Any remaining renders are attributed separately from store subscription behavior. |

Compare committed work after initial mount; development Strict Mode and selector checks may call render or selector functions extra times. Do not infer performance from selector call counts or claim measured improvement without evidence. Keep probes temporary unless a regression test has lasting value.

When reporting the change, identify the store and subscriptions changed, the update paths checked, and any remaining reason for renders or memoization. If runtime verification was unavailable, say so.
