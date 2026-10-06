# Redux React Hooks, Middleware, and Async Work

## React Redux hooks

Wrap the React application in `<Provider store={store}>` once near its root so nested components can access the Redux store.

- **`useDispatch()`** returns the store's dispatch function. Components dispatch actions in response to user events.
- **`useSelector(selector)`** subscribes to the store and returns the selected value. By default React Redux compares the previous and next result with strict reference equality; selectors should return stable values when practical.

```jsx
import { useDispatch, useSelector } from "react-redux";
import { increment } from "./counter-slice.js";

function Counter() {
  const value = useSelector((state) => state.counter.value);
  const dispatch = useDispatch();
  return <button onClick={() => dispatch(increment())}>{value}</button>;
}
```

Avoid returning a new object or array from an inline selector on every store update unless using a memoized selector or an appropriate equality function; otherwise the component can re-render unnecessarily.

## Middleware

Middleware sits between dispatching an action and the reducer. It can inspect, log, delay, transform, or handle dispatched values. Middleware is commonly used for async workflows, analytics, and cross-cutting behavior.

Redux Toolkit installs thunk middleware by default. A thunk is a function that can dispatch actions over time and perform asynchronous work; reducers themselves remain synchronous and pure.

## `createAsyncThunk`

`createAsyncThunk` creates a thunk action and lifecycle actions: `pending`, `fulfilled`, and `rejected`. Handle those in a slice's `extraReducers`, keeping loading/error status in state.

```javascript
import { createAsyncThunk, createSlice } from "@reduxjs/toolkit";

export const loadUser = createAsyncThunk(
  "users/loadUser",
  async (userId, { rejectWithValue }) => {
    const response = await fetch(`/api/users/${userId}`);
    if (!response.ok) return rejectWithValue("Could not load user");
    return response.json();
  }
);

const userSlice = createSlice({
  name: "user",
  initialState: { data: null, status: "idle", error: null },
  reducers: {},
  extraReducers(builder) {
    builder
      .addCase(loadUser.pending, (state) => {
        state.status = "loading";
        state.error = null;
      })
      .addCase(loadUser.fulfilled, (state, action) => {
        state.status = "succeeded";
        state.data = action.payload;
      })
      .addCase(loadUser.rejected, (state, action) => {
        state.status = "failed";
        state.error = action.payload ?? action.error.message;
      });
  }
});
```

Production API handling should also consider request cancellation, stale responses, retries, and whether this data belongs in a query cache. For standard server-state fetching/caching, evaluate RTK Query rather than hand-building the same lifecycle for every endpoint.

## Interview reminders

Explain which code runs in middleware vs. reducers, how `createAsyncThunk` models request states, how a component subscribes with `useSelector`, and why a selector returning a new reference can trigger extra renders.
