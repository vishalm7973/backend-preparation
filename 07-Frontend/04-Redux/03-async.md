# Redux React Hooks, Middleware, and Async Work

## 1. React Redux Hooks

Wrap the app with `Provider` so components can access the Redux store.

    <Provider store={store}>
      <App />
    </Provider>

### `useSelector`

Used to **read data from the Redux store**.

    const value = useSelector(
      (state) => state.counter.value
    );

### `useDispatch`

Used to **dispatch actions**.

    const dispatch = useDispatch();

    dispatch(increment());

### Remember

    useSelector → Read Redux state
    useDispatch → Send actions

---

## 2. Middleware

Middleware runs **between dispatching an action and the reducer**.

    dispatch(action)
          ↓
      Middleware
          ↓
       Reducer
          ↓
      New State

Middleware can be used for:

- Async operations
- Logging
- Analytics
- Other common application logic

Redux Toolkit includes **thunk middleware by default**.

### Remember

> Middleware → Runs between `dispatch` and the reducer.

Reducers should remain **pure and synchronous**.

---

## 3. `createAsyncThunk`

`createAsyncThunk` is used for common **async operations**, such as API calls.

It automatically provides three states:

    pending   → Request started
    fulfilled → Request succeeded
    rejected  → Request failed

Example:

    export const loadUser = createAsyncThunk(
      "users/loadUser",
      async (userId) => {
        const response = await fetch(`/api/users/${userId}`);
        return response.json();
      }
    );

Handle these states inside `extraReducers`:

    extraReducers(builder) {
      builder
        .addCase(loadUser.pending, (state) => {
          state.status = "loading";
        })
        .addCase(loadUser.fulfilled, (state, action) => {
          state.status = "succeeded";
          state.data = action.payload;
        })
        .addCase(loadUser.rejected, (state) => {
          state.status = "failed";
        });
    }

---

## 4. Selector Important Point

`useSelector` checks whether the selected result changed.

Avoid creating a new object/array unnecessarily:

    // Can cause unnecessary re-renders
    useSelector((state) => ({
      name: state.user.name
    }));

Prefer selecting the value directly:

    useSelector((state) => state.user.name);

---

### Interview Answer

> `useSelector` reads data from the Redux store, while `useDispatch` sends actions. Middleware runs between dispatch and the reducer and is commonly used for async work. `createAsyncThunk` simplifies API requests by providing pending, fulfilled, and rejected states.