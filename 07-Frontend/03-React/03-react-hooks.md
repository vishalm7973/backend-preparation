# React Hooks

Hooks let function components use React features like **state, effects, context, and refs**.

---

## 1. Rules of Hooks

- Call Hooks only at the **top level**.
- Do not call Hooks inside loops, conditions, or nested functions.
- Call Hooks only from React components or custom Hooks.
- Keep Hook calls in the **same order** on every render.

### Wrong

    if (isLoggedIn) {
      useEffect(() => {});
    }

### Correct

    useEffect(() => {
      if (isLoggedIn) {
        // logic
      }
    }, [isLoggedIn]);

---

## 2. `useState`

Used to store component state.

    const [count, setCount] = useState(0);

    setCount(10);

When state changes, React schedules a re-render.

### Previous State

If the new value depends on the previous value, use a functional updater.

    setCount((current) => current + 1);

### Important

State is a **snapshot for the current render**.

Do not directly mutate objects or arrays in state.

    user.name = "John"; // ❌

Create a new object instead.

    setUser({
      ...user,
      name: "John"
    }); // ✅

---

## 3. `useEffect`

Used to **synchronize with external systems** such as:

- API calls
- Timers
- Event listeners
- Subscriptions
- Browser APIs

    useEffect(() => {
      console.log("Effect runs");

      return () => {
        console.log("Cleanup");
      };
    }, []);

Effects run **after the render is committed**.

### Example

    useEffect(() => {
      const timer = setInterval(() => {
        console.log("Running");
      }, 1000);

      return () => {
        clearInterval(timer);
      };
    }, []);

### Remember

    useEffect → Synchronize with external systems

---

## 4. `useEffect` Dependency Array

### No dependency array

Runs after every commit.

    useEffect(() => {
      console.log("Runs after every render");
    });

### Empty array `[]`

Runs after the component mounts.

Cleanup runs when the component unmounts.

    useEffect(() => {
      console.log("Mounted");

      return () => {
        console.log("Unmounted");
      };
    }, []);

### Dependencies

Runs after mount and when a dependency changes.

    useEffect(() => {
      console.log(userId);
    }, [userId]);

### Cleanup

Cleanup runs before the effect runs again and when the component unmounts.

    useEffect(() => {
      const timer = setInterval(() => {
        console.log("Running");
      }, 1000);

      return () => {
        clearInterval(timer);
      };
    }, []);

### Remember

    []        → No reactive dependencies
    [value]   → Runs when value changes
    no []     → Runs after every commit

---

## 5. `useContext`

Used to read data from React Context.

It helps avoid passing props through many components (**prop drilling**).

    const theme = useContext(ThemeContext);

Example:

    const ThemeContext = createContext("light");

    function App() {
      return (
        <ThemeContext.Provider value="dark">
          <Dashboard />
        </ThemeContext.Provider>
      );
    }

    function Dashboard() {
      const theme = useContext(ThemeContext);

      return <p>{theme}</p>;
    }

### Remember

    useContext → Read shared context value

---

## 6. `useRef`

Used to store a value that **persists between renders without causing a re-render**.

It is also commonly used to directly access a DOM element, such as an input.

    const inputRef = useRef(null);

    <input ref={inputRef} />

    inputRef.current?.focus();

### Example

Store a value without triggering a render:

    const countRef = useRef(0);

    countRef.current++;

### Remember

    useState → Changing value causes re-render
    useRef   → Changing .current does NOT cause re-render

---

## 7. `useMemo`

Caches the **result of a calculation**.

    const filteredUsers = useMemo(
      () => users.filter((user) => user.active),
      [users]
    );

The calculation runs again when `users` changes.

### Remember

    useMemo → Memoizes a value/result

Use it mainly when a calculation is expensive or stable value identity matters.

---

## 8. `useCallback`

Caches a **function** between renders.

    const handleSave = useCallback(() => {
      save(userId);
    }, [userId]);

### Remember

    useMemo     → Memoizes a value
    useCallback → Memoizes a function

It is mainly useful when function identity matters, such as when passing a callback to a memoized child.

---

## 9. `React.memo`

`React.memo` is used to **prevent unnecessary re-renders of a component** when its props have not changed.

    const User = React.memo(function User({ name }) {
      return <p>{name}</p>;
    });

If the parent re-renders but `name` is the same, React can skip re-rendering `User`.

### Important

`React.memo` checks props using **shallow comparison** by default.

For example:

    <User name="John" />

If the prop value stays the same, the child can skip rendering.

But with objects/functions:

    <User user={{ name: "John" }} />

A new object is created on every render, so the prop is considered changed.

### `React.memo` + `useCallback`

Useful when passing functions to memoized children.

    const Child = React.memo(({ onSave }) => {
      return <button onClick={onSave}>Save</button>;
    });

    function Parent() {
      const handleSave = useCallback(() => {
        console.log("Saved");
      }, []);

      return <Child onSave={handleSave} />;
    }

### Remember

    React.memo   → Memoizes a component
    useMemo      → Memoizes a value
    useCallback  → Memoizes a function

Use `React.memo` when avoiding unnecessary child re-renders is actually useful. It is a **performance optimization**, not something every component needs.

---

## 10. `useReducer`

Useful for **complex state logic**.

    function reducer(state, action) {
      switch (action.type) {
        case "increment":
          return {
            count: state.count + 1
          };

        case "reset":
          return {
            count: 0
          };

        default:
          return state;
      }
    }

    const [state, dispatch] = useReducer(
      reducer,
      { count: 0 }
    );

### Main Parts

- **State** → Current data of the component.
- **Action** → Describes what happened / what change we want.
- **Reducer** → Function that decides how the state should change.
- **Dispatch** → Sends an action to the reducer.

### Flow

    dispatch(action)
          ↓
       reducer
          ↓
      new state
          ↓
    re-render

### Remember

    useState   → Simple state
    useReducer → Complex state transitions

Reducers should be **pure** and should return new state.

---

## 11. Custom Hooks

A custom Hook is a function whose name starts with `use`.

It allows us to **reuse stateful logic** between components.

    function useCounter() {
      const [count, setCount] = useState(0);

      const increment = () => {
        setCount((current) => current + 1);
      };

      return {
        count,
        increment
      };
    }

Use it:

    function App() {
      const { count, increment } = useCounter();

      return (
        <button onClick={increment}>
          {count}
        </button>
      );
    }

Each call to a custom Hook has its **own state**.