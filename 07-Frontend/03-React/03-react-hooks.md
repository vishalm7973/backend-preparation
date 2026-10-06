# React Hooks

Hooks let function components use React features such as state, context, refs, and effects.

## Rules of Hooks

- Call Hooks only at the top level of a function component or a custom Hook, not inside loops, conditions, or nested functions.
- Call Hooks only from React function components or custom Hooks.
- Keep Hook calls in the same order on every render so React can associate state with each call.

## `useState`

Stores component state and returns the current render's value plus a setter. The setter schedules another render; use a functional updater when the next value depends on the previous one.

```jsx
const [count, setCount] = useState(0);
setCount((current) => current + 1);
```

State is a snapshot for that render. Mutating an object in state and passing the same reference may not cause the update you expect; create a new object/array.

## `useEffect`

Synchronizes a component with an external system, such as a subscription, timer, or browser API. Effects run after a commit. Do not use an effect for a value that can be calculated during render or for every user event when the event handler can perform the work directly.

```jsx
useEffect(() => {
  const connection = connect(roomId);
  return () => connection.disconnect();
}, [roomId]);
```

## Effect dependency array and cleanup

React compares dependency values using `Object.is` to decide when an effect should re-synchronize. Include every reactive value read by the effect; omitting one can create stale closures. Restructure the effect or memoize inputs only when there is a clear reason rather than suppressing dependency warnings.

- No dependency array: effect runs after every commit.
- `[]`: effect has no reactive dependencies; setup runs after mount and cleanup on unmount, with development Strict Mode checks possibly repeating setup/cleanup.
- `[a, b]`: effect re-runs when a dependency changes.
- A cleanup function runs before the effect re-runs and on unmount. Use it to unsubscribe, clear timers, or abort obsolete requests.

## `useContext`

Reads the nearest value provided for a context. It helps avoid passing a value through many intermediate components. Consumers update when the provider value changes; context is not automatically a replacement for every global state store.

```jsx
const theme = useContext(ThemeContext);
```

## `useRef`

Returns a stable object whose `.current` value persists between renders. Changing `.current` does not trigger a render. Use refs for DOM nodes or mutable values that do not determine rendered output.

```jsx
const inputRef = useRef(null);
// <input ref={inputRef} />
inputRef.current?.focus();
```

## `useMemo`

Caches the result of a calculation between renders while dependencies are unchanged. Use it for measured expensive calculations or stable derived values; it is a performance optimization, not a semantic guarantee.

```jsx
const visibleItems = useMemo(
  () => filterItems(items, query),
  [items, query]
);
```

## `useCallback`

Caches a function definition between renders while dependencies are unchanged. It is mainly useful when function identity matters to a memoized child or another Hook; do not add it by default.

```jsx
const handleSave = useCallback(() => save(recordId), [recordId]);
```

## `useReducer`

Manages state through a reducer function that receives the current state and an action and returns the next state. It is useful when transitions are related, involve multiple fields, or benefit from centralized action logic.

```jsx
function reducer(state, action) {
  switch (action.type) {
    case "increment": return { count: state.count + 1 };
    case "reset": return { count: 0 };
    default: return state;
  }
}
const [state, dispatch] = useReducer(reducer, { count: 0 });
```

Reducers should be pure and should return new state rather than mutate existing state.

## Custom Hooks

A custom Hook is a function whose name starts with `use` and that composes built-in or other custom Hooks. It shares stateful logic between components; each call has its own Hook state unless it reads shared state such as context or an external store.

```jsx
function useOnlineStatus() {
  const [online, setOnline] = useState(navigator.onLine);
  useEffect(() => {
    const update = () => setOnline(navigator.onLine);
    window.addEventListener("online", update);
    window.addEventListener("offline", update);
    return () => {
      window.removeEventListener("online", update);
      window.removeEventListener("offline", update);
    };
  }, []);
  return online;
}
```

## Interview reminders

Explain Hook call ordering, state snapshots, effect synchronization and cleanup, dependency correctness, when a ref differs from state, and why memoization should be used to address a measured identity or computation cost.
