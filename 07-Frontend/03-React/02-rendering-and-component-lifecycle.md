# React Rendering and Component Lifecycle

## How React Renders Components

At a high level, React calls components to produce an element tree, compares that result with the previous tree (reconciliation), then commits necessary changes to the host environment such as the browser DOM.

```text
Props / state / context change
             |
             v
       Render components
             |
             v
 Reconcile previous and next trees
             |
             v
 Commit necessary DOM updates
```

Rendering means calculating the next UI description; it does not mean the browser DOM is fully rebuilt. Keep render logic pure and avoid side effects during render.

## Re-rendering

A component may render again when its state changes, its parent renders with new inputs, a context it reads changes, or a subscribed external store notifies it. A render does not guarantee a DOM mutation: React can determine the output is unchanged.

- State setters schedule updates; React may batch multiple updates.
- Parent renders can cause child components to render again unless an optimization or unchanged component boundary prevents it.
- `memo`, `useMemo`, and `useCallback` are performance tools, not correctness mechanisms; profile before using them broadly.

## Mounting, Updating, and Unmounting

- **Mount:** A component is added to the rendered tree.
- **Update:** Its props, state, context, or relevant external subscription changes.
- **Unmount:** It is removed from the tree; cleanup is needed for subscriptions, timers, and other external resources.

In function components, an effect runs after a committed render when its dependencies change. Its cleanup runs before that effect re-runs and when the component unmounts. This models synchronization with external systems, not a one-to-one replacement for every class lifecycle method.

## Component Lifecycle

Class components expose lifecycle methods such as `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount`. Modern React commonly uses function components and Hooks instead. Learn the phases and cleanup behavior rather than memorizing lifecycle methods as the primary API.

In development Strict Mode, React may re-run render-related logic and effect setup/cleanup to help reveal impure code and missing cleanup. Do not assume an effect setup runs exactly once in every environment.

## Virtual DOM and Diffing

The **Virtual DOM** is a common shorthand for React's in-memory element descriptions. React elements are not a literal copy of the browser DOM. During reconciliation, React compares the previous and next element trees and determines which host updates are needed.

“Diffing” describes this comparison informally. The exact internal algorithms are implementation details; interview answers should focus on identity, element type, keys, and the resulting update behavior.

## Reconciliation

Reconciliation is React's process for matching the new element tree with the previous one. If element types remain compatible, React can preserve component state and update changed props/children. If identity or type changes, React may remove the old subtree and mount a new one.

Changing a component's `key` intentionally changes its identity and resets its local state. This can be useful for resetting a form when switching between records.

## Keys and Why They Matter

Keys give elements in a list stable identity across renders. They help React match items when elements are inserted, removed, or reordered.

```jsx
{users.map((user) => (
  <UserRow key={user.id} user={user} />
))}
```

- Use a stable unique ID from the data when possible.
- Avoid array indices when the list can reorder, insert, or remove items; state or DOM may be associated with the wrong item.
- Avoid generating a new random key on every render; it remounts the item each time.
- Keys are special React metadata and are not passed to the component as a normal prop.

## Interview reminders

Trace a render through render, reconcile, and commit; explain what triggers re-renders; distinguish component lifecycle concepts from effect dependencies; and explain how keys preserve identity in changing lists.
