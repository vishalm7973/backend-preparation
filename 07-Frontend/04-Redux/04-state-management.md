# Choosing Redux, Context, or Local State

State should live at the narrowest level that can correctly serve its consumers. Avoid globalizing state just because a state-management library is available.

## Local state

Use component-local state for UI data owned by one component or a small subtree: a menu's open state, an input draft, or a selected tab. `useState` handles simple transitions; `useReducer` can help when related transitions are clearer as named actions.

## Lifting state up

When sibling components need the same value, move its ownership to their nearest common parent and pass the value and callbacks down. This keeps one source of truth without introducing a global store.

## Prop drilling

Prop drilling means forwarding props through components that do not use them so a deeper descendant can access them. It is not automatically bad: for a short and explicit component tree, props keep dependencies visible. Consider composition or Context when the pass-through becomes cumbersome or broadly shared.

## Context API

Context transports a value through a component tree without forwarding it at every level. It is a good fit for cross-cutting, relatively stable values such as theme, locale, or current-user metadata. A changed provider value updates consumers that read it; Context does not automatically provide Redux-style actions, reducers, middleware, or debugging history.

## Redux

Redux is useful when substantial client-owned state is shared across distant parts of the application, transitions are complex, or centralized tooling and debugging are valuable. Redux Toolkit is the recommended way to write Redux.

For server-owned data, consider a query/cache layer such as RTK Query rather than treating every fetched response as generic global client state.

## Redux vs. Context API

- Choose **Context** for dependency-like values or relatively simple shared state that changes at a manageable rate.
- Choose **Redux** when shared client state has complex transitions, many consumers, or benefits from middleware, selectors, DevTools, and explicit update conventions.
- They are not mutually exclusive; an application can use Redux for domain state and Context for a small dependency such as theme.

## Redux vs. local state

- Keep state **local** when one component or subtree owns it; this limits coupling and unnecessary global updates.
- Use **Redux** when multiple distant features need to coordinate around the same client state or centralized state transitions are valuable.
- Do not copy derived values into either local or global state when they can be calculated from existing state.

## When to use global state

Before adding a global store, ask:

1. Is this client/UI state, or server state with its own cache and freshness rules?
2. Which components need to read and update it?
3. Can it be lifted to a common parent or supplied through Context?
4. Are the state transitions or debugging requirements complex enough to justify Redux?
5. Can unnecessary state be derived instead of stored?

## Interview answer

Start with local state, lift it when sibling components must coordinate, and use Context for values shared through a tree. Choose Redux when complex client state is shared broadly enough to justify a centralized store and its tooling. Explain the tradeoffs rather than saying one solution always scales best.
