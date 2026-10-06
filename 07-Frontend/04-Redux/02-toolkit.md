# Redux Toolkit

**Redux Toolkit (RTK)** is the recommended way to write Redux. It includes utilities for store setup, slices, immutable updates, async workflows, and server-data caching.

## `configureStore`

`configureStore` creates the store, combines reducer maps, installs useful development checks, and includes thunk middleware by default.

## `createSlice`

`createSlice` groups a feature's name, initial state, and reducer functions. It generates action creators and action types automatically. Reducers may use mutation-like syntax because RTK uses Immer to produce immutable updates; do not mutate state outside these reducers.

```javascript
import { configureStore, createSlice } from "@reduxjs/toolkit";

const counterSlice = createSlice({
  name: "counter",
  initialState: { value: 0 },
  reducers: {
    increment(state) {
      state.value += 1;
    },
    incrementBy(state, action) {
      state.value += action.payload;
    }
  }
});

export const { increment, incrementBy } = counterSlice.actions;

const store = configureStore({
  reducer: { counter: counterSlice.reducer }
});

store.dispatch(incrementBy(2));
```

The apparent mutation is an Immer draft; RTK creates the next immutable state. Action creators return ordinary actions, for example `{ type: "counter/incrementBy", payload: 2 }`.

## Store and React setup

Use React Redux's `Provider` to make the store available to the component tree. Components then read state with `useSelector` and dispatch actions with `useDispatch` (covered in [React Redux hooks and async](03-async.md)).

```jsx
import { Provider } from "react-redux";

root.render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

## Toolkit interview points

- Prefer feature slices over hand-writing action type constants and switch-based reducers for every feature.
- Keep state serializable when possible so DevTools, replay, and persistence behave predictably. Avoid storing functions, class instances, DOM nodes, or Promises in Redux state.
- `createAsyncThunk` handles common request lifecycles; RTK Query is a higher-level option for server-data fetching and caching.
- Split reducers by feature as the application grows; `configureStore` combines them.
