# Redux Toolkit

**Redux Toolkit (RTK)** is the recommended way to write Redux.

It makes Redux easier by providing tools for:
- Store setup
- Slices
- Reducers
- Async operations
- Server-data caching

---

## 1. `configureStore`

Creates the Redux store.

It also:
- Configures reducers
- Adds useful middleware
- Enables development checks
- Includes thunk middleware by default

    const store = configureStore({
      reducer: {
        counter: counterSlice.reducer
      }
    });

### Remember

    configureStore → Create and configure the Redux store

---

## 2. `createSlice`

`createSlice` creates a feature with:

- Name
- Initial state
- Reducers

It automatically creates **actions and action types**.

    const counterSlice = createSlice({
      name: "counter",

      initialState: {
        value: 0
      },

      reducers: {
        increment(state) {
          state.value += 1;
        },

        incrementBy(state, action) {
          state.value += action.payload;
        }
      }
    });

    export const {
      increment,
      incrementBy
    } = counterSlice.actions;

### Dispatch

    store.dispatch(incrementBy(2));

RTK uses **Immer**, so mutation-like code inside reducers is safe.

    state.value += 1;

Immer creates the new immutable state internally.

### Remember

    createSlice → State + Reducers + Actions

---

## 3. Provider

`Provider` makes the Redux store available to React components.

    import { Provider } from "react-redux";

    root.render(
      <Provider store={store}>
        <App />
      </Provider>
    );

Then components can use:

    useSelector() → Read state
    useDispatch() → Dispatch actions

---

## 4. Redux Toolkit Async

### `createAsyncThunk`

Used for common async operations such as API requests.

    createAsyncThunk → Handle async request lifecycle

It provides states like:

    pending
    fulfilled
    rejected

### RTK Query

Used for **API data fetching and caching**.

    RTK Query → Fetch + Cache server data

---

## 5. Keep Redux State Serializable

Prefer storing simple data:

    string
    number
    boolean
    array
    object

Avoid storing:

    functions
    DOM elements
    Promises
    class instances

This keeps Redux DevTools and persistence predictable.

---

## 6. Feature-Based Structure

As the application grows, split Redux code by feature.

    store
      ├── counter
      │   └── counterSlice.js
      │
      ├── users
      │   └── userSlice.js
      │
      └── products
          └── productSlice.js

---

### Interview Answer

> Redux Toolkit is the recommended way to use Redux. It reduces boilerplate using `configureStore` and `createSlice`, simplifies immutable updates with Immer, and provides tools like `createAsyncThunk` and RTK Query for async and server data.