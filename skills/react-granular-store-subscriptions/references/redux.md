# Redux and React Redux

## Example versions

| Scope | API target |
| --- | --- |
| React baseline | React 18.x function components, with TypeScript/TSX |
| React bindings | `react-redux` 9.x |
| State and derived selectors | `@reduxjs/toolkit` 2.x, with Redux 5.x and Reselect 5.x |

Documentation reviewed: **2026-09-23**, using the official [RTK 2 / Redux 5 migration guide](https://redux.js.org/usage/migrations/migrating-rtk-2) and [React Redux TypeScript guide](https://react-redux.js.org/using-react-redux/usage-with-typescript).

Validation status: documentation-checked, illustrative snippets; no runtime test matrix or exact tested patch versions. These targets identify the API baseline, not a requirement to change the consuming project's dependencies. Check its resolved versions and peer dependencies before adapting the examples.

The examples reuse project-defined typed hooks. If defining them with `.withTypes()`, that API requires React Redux 9.1.0 or later; earlier versions can use explicitly typed hooks. Reselect 5 changes the default memoizer, so do not carry cache-size assumptions from older releases into parameterized selectors.

## Subscription behavior

Reuse the project's typed selector/dispatch hooks and the intended `Provider`. `useSelector` compares results with `===` by default; a new object or array can trigger a render even when its contents are equivalent. Multiple small selectors are valid. See [React Redux hooks](https://react-redux.js.org/api/hooks).

## Select where the value is rendered

Assume the project already exports `useAppSelector` and stores tasks as stable IDs plus entities. Adapt paths and state shape to the project.

```tsx
import { useAppSelector } from './store/hooks';

export function TaskList() {
  const ids = useAppSelector((state) => state.tasks.ids);

  return (
    <ul>
      {ids.map((id) => <TaskTitle key={id} id={id} />)}
    </ul>
  );
}

function TaskTitle({ id }: { id: string }) {
  const title = useAppSelector((state) => state.tasks.entities[id]?.title);

  if (title === undefined) return null;
  return <li>{title}</li>;
}
```

Avoid selecting `state`, all of `state.tasks`, or all entities in the parent just to pass individual titles down. An unrelated field update should preserve `tasks.ids`. An actual membership/order change still renders the parent and may render its children.

An action-only button needs `useAppDispatch`, not a state subscription. Do not substitute `store.getState()` for reactive rendering; it does not subscribe.

## Narrow before computing

This still subscribes to every replacement of the entity map, despite caching the title lookup:

```tsx
import { useMemo } from 'react';

// Inside a component:
const entities = useAppSelector((state) => state.tasks.entities);
const title = useMemo(() => entities[id]?.title, [entities, id]);
```

Replace both lines with the subscription the component actually needs:

```tsx
const title = useAppSelector((state) => state.tasks.entities[id]?.title);
```

The same principle applies to other selector-based stores: downstream memoization cannot change what the subscription observes.

## Combine values only when useful

Prefer separate primitive selections. For a small grouped result, the supported shallow comparison is:

```tsx
import { shallowEqual } from 'react-redux';

// Inside a component, using the project's typed hook:
const { title, completed } = useAppSelector(
  (state) => ({
    title: state.tasks.entities[id]?.title,
    completed: state.tasks.entities[id]?.completed,
  }),
  shallowEqual,
);
```

Nested freshly allocated values still fail shallow comparison. Equality must account for every selected value the component uses.

## Derived collections

For a cached derived result in a Redux Toolkit project:

```ts
import { createSelector } from '@reduxjs/toolkit';
import type { RootState } from './store';

export const selectOpenTaskIds = createSelector(
  [
    (state: RootState) => state.tasks.ids,
    (state: RootState) => state.tasks.entities,
  ],
  (ids, entities) => ids.filter((id) => entities[id]?.completed === false),
);
```

Define reusable selectors outside renders. Keep input selectors as stable lookups. This selector reuses its result only while its inputs remain equal; replacing the entity map recomputes it and produces a new array even if membership stays the same. If that matters, assess a supported result equality strategy or narrower inputs. Do not add `createSelector` to plain field lookups. Adapt parameterized selector cache ownership to the installed Reselect version and actual usage. See [Redux selector guidance](https://redux.js.org/usage/deriving-data-selectors).
