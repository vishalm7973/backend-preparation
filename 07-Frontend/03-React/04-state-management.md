# React State Management

Choose a state location based on who needs the data, how it changes, and whether it is local UI state, shared client state, or server-owned data.

## Local state

Keep state in the component that owns the interaction when only that component or its immediate UI needs it. Examples include whether a menu is open, a draft input, and the selected tab.

Use `useState` for simple transitions and `useReducer` when a component has related transitions that are clearer as actions.

## Lifting state up

When sibling components need to read or change the same state, move it to their nearest common parent and pass the value and event callbacks through props. This creates one source of truth for that piece of UI.

```text
Parent owns selected item
   |-- Child A receives value + onSelect
   `-- Child B receives value + onSelect
```

Lift only as high as necessary; state kept too high can cause unrelated subtrees to update and makes ownership harder to understand.

## Prop drilling

Prop drilling is passing data through intermediate components that do not use it so a deeper component can receive it. For a short, clear component chain, props are explicit and often the simplest option. For broad cross-cutting values, consider composition or Context.

## Context API

Context provides a value to descendants without explicitly forwarding it through every intermediate component. It works well for relatively stable cross-tree values such as theme, locale, or current authenticated user metadata.

```jsx
const ThemeContext = createContext("light");

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Toolbar />
    </ThemeContext.Provider>
  );
}
```

Context is a transport mechanism, not a complete state-management architecture. Consumers that read a changed provider value re-render; split contexts or stabilize provider values when profiling shows unnecessary updates.

## Redux

Redux stores shared client state in a centralized store. Components dispatch actions; reducers calculate the next state; selectors read derived values. Redux's predictable transitions and tooling can help when many parts of an application share complex state.

## Redux Toolkit

Redux Toolkit (RTK) is the recommended way to write Redux in modern applications. It reduces boilerplate through `configureStore`, `createSlice`, and built-in Immer-based immutable update handling. RTK Query can manage server-data fetching and caching when that fits the application.

Redux is not automatically needed just because an application uses React. Local state and Context can be sufficient for smaller or simpler sharing needs.

## Context vs. Redux

| Context | Redux / Redux Toolkit |
|---|---|
| Built into React; supplies values through a tree | External state-management library with a store and explicit update flow |
| Good for dependency-like or relatively stable cross-cutting values | Useful for complex shared client state, many coordinated updates, and richer debugging/middleware needs |
| No built-in action/reducer conventions or state tooling | Offers actions, reducers, selectors, middleware, and DevTools integration |
| Consumers update when a value they read changes | Components can subscribe to selected store data through bindings |

Neither is universally better. Consider update frequency, state complexity, team familiarity, debugging needs, and bundle/operational cost.

## When to use global state

Use a shared/global store when multiple distant parts of the app need the same client-owned state or when transitions and debugging are complex enough to justify centralized rules. Keep state local when it is only used by one feature or component subtree.

Before globalizing state, ask:

- Is this UI/client state or server-owned data that belongs in a query cache?
- Which components read or update it?
- Can it be derived from existing state instead of duplicated?
- How often does it change, and what should update when it does?

## Interview answer

Start with local state. Lift it to the nearest common parent when siblings need to coordinate. Use Context for cross-cutting values and a store such as Redux Toolkit when shared client state, transitions, and debugging justify it. Treat server state separately when a query/cache library is appropriate.
