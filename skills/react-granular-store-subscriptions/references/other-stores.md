# Other React Store Models

Apply the same goal using the installed library's native change tracking. A selector hook is not the right abstraction for every store. Verify library and adapter versions before using an unfamiliar API.

## Jotai and other atom stores

Subscribe to an atom representing the value the component needs. In Jotai, use `useAtomValue` for reads and `useSetAtom` for write-only controls. Reading a large object atom and destructuring it still subscribes to that atom. Consider smaller or derived atoms for independently changing data. Keep atom identities stable; follow existing patterns for parameterized atoms instead of constructing a new atom unconditionally in every render.

See [Jotai performance guidance](https://jotai.org/docs/guides/performance).

## MobX and observable stores

Read the required observable properties inside a small `observer` component so tracking happens at that boundary. Pass observable objects down and dereference them where displayed; reading properties in an ancestor makes that ancestor a consumer. Keep MobX's native action and computed conventions. `observer` already applies React memoization for function components, so an extra blanket `React.memo` wrapper is unnecessary.

See [MobX React integration](https://mobx.js.org/react-integration.html).

## Valtio and proxy stores

Use `useSnapshot` in the component that reads the needed snapshot properties. Read from the snapshot during rendering and write through the original proxy in handlers. Broad enumeration or serialization of a snapshot reads more properties and can expand tracking. Do not replace a scoped proxy instance during an unrelated render.

See [Valtio useSnapshot](https://valtio.dev/docs/api/basic/useSnapshot).

## React Context

Native `useContext` subscribes to the provider value as a whole. Destructuring one property or wrapping the consumer in `React.memo` does not isolate it from changes to that context value. Split contexts by independent concerns when appropriate, or pass a stable store instance through context and subscribe through its existing React adapter. Stabilizing a provider value avoids accidental identity changes; it does not create per-property subscriptions.

See [React useContext](https://react.dev/reference/react/useContext).

## Custom external stores and other adapters

Prefer the store's maintained React adapter. If implementing a custom adapter is actually in scope, use `useSyncExternalStore` with a stable subscription function and cleanup. Return cached, immutable snapshots whose identity changes when the observed data changes. Supply a consistent server snapshot when server rendering applies. A full-store snapshot still exposes whole-store updates; use a supported selector adapter or a correctly cached selected snapshot for narrower observations.

Do not emulate subscriptions with an effect plus ad hoc local state, and do not read changing external state directly during render without a subscription. See [React useSyncExternalStore](https://react.dev/reference/react/useSyncExternalStore).

For another state library, identify its observation unit, equality semantics, write API, and instance lifetime. Use the smallest useful observation unit without changing libraries solely to mimic Redux or Zustand.
