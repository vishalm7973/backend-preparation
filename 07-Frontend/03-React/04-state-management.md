# React State Management

State management means deciding **where state should live** and **who needs it**.

---

## 1. Local State

Use local state when only one component or a small part of the UI needs the data.

Examples:
- Menu open/close
- Form input
- Selected tab

Remember:

    useState → Simple state
    useReducer → Complex state logic

---

## 2. Lifting State Up

When multiple sibling components need the same state, move it to their **nearest common parent**.

    Parent owns state
       |
       |-- Child A → receives value + onChange
       |
       └-- Child B → receives value + onChange

This creates **one source of truth**.

### Remember

Lift state only as high as necessary.

---

## 3. Prop Drilling

Prop drilling means passing props through components that **do not need the data**, just to reach a deeper component.

    Parent
      ↓ props
    Child
      ↓ props
    GrandChild
      ↓ props
    Target

For a small component tree, props are usually fine.

For data needed across many components, consider **Context** or a state-management library.

---

## 4. Context API

Context lets components access shared data without passing props through every level.

Common uses:
- Theme
- Language
- User information

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

    Context → Share data across a component tree

Context is **not a complete state-management solution** like Redux.

---

## 5. Redux

Redux provides a **central store** for shared client-side state.

### Basic Flow

    Component
        ↓
    dispatch(action)
        ↓
    Reducer
        ↓
    New State
        ↓
    Store
        ↓
    Component updates

### Main Parts

- **Store** → Holds application state.
- **Action** → Describes what happened.
- **Reducer** → Calculates the new state.
- **Dispatch** → Sends an action.
- **Selector** → Reads data from the store.

---

## 6. Redux Toolkit

**Redux Toolkit (RTK)** is the recommended way to write Redux.

Common APIs:

    configureStore → Create store
    createSlice    → Create state + reducers
    useSelector    → Read state
    useDispatch    → Dispatch actions

RTK reduces Redux boilerplate and makes Redux easier to use.

---

## 7. Context vs Redux

| Context | Redux / RTK |
|---|---|
| Built into React | External library |
| Good for shared values | Good for complex shared state |
| Simple setup | More structured |
| No built-in actions/reducers | Actions + reducers + selectors |
| Good for theme, locale, user info | Good for large/complex client state |

Neither is always better.

---

## 8. When to Use Global State

Use global/shared state when:

- Many different parts of the app need the same data.
- State updates are complex.
- Centralized debugging is useful.

Keep state **local** when only one component or feature needs it.

Before making state global, ask:

    Who needs this data?
    How often does it change?
    Can I keep it local?
    Can I derive it instead of storing it?
    Is it server data or client/UI data?

---

## Interview Answer

> Start with local state. If sibling components need the same state, lift it to their nearest common parent. Use Context for shared values, and Redux Toolkit when the application has complex shared client state that benefits from centralized state and debugging.