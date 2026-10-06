# Redux Basics

Redux is a predictable state-management library. It keeps shared application state in a store and changes that state through explicit actions and reducers. It is useful when state is shared across distant components, has coordinated transitions, or benefits from centralized debugging.

## Core concepts

- **Store:** Holds the current Redux state tree and provides `getState`, `dispatch`, and `subscribe` behavior. In modern applications, create it with Redux Toolkit's `configureStore`.
- **State:** The current data in the store. Treat it as read-only; only reducers calculate the next state.
- **Action:** A plain object describing what happened, usually with a `type` and optional `payload`.
- **Reducer:** A pure function of `(previousState, action)` that returns the next state. It should not perform I/O, mutate external data, or depend on time/randomness.
- **Dispatch:** Sends an action through the store's middleware and reducer pipeline.
- **Selector:** A function that reads or derives a value from the state tree.

## Redux data flow

```text
UI event -> dispatch(action) -> reducer calculates next state
   ^                                      |
   |                                      v
   +--- selector reads updated state <- store notifies subscribers
```

The flow is one-way. A view dispatches an intent; reducers compute the state transition; subscribed UI reads the new state.

## Plain action and reducer example

```javascript
const incremented = { type: "counter/incremented" };

function counterReducer(state = { value: 0 }, action) {
  switch (action.type) {
    case "counter/incremented":
      return { ...state, value: state.value + 1 };
    default:
      return state;
  }
}
```

This illustrates the Redux model. In production Redux applications, Redux Toolkit's `createSlice` and `configureStore` reduce boilerplate and are the recommended starting point.

## Interview reminders

Explain the action -> reducer -> store -> selector flow, why reducers are pure, and why Redux is for shared client state rather than a requirement for every React component. For Toolkit setup and generated actions, see [Redux Toolkit](02-toolkit.md).
