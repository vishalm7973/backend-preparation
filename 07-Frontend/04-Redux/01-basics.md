# Redux Basics

Redux is a **state-management library** used to manage shared application state.

It is useful when many components need the same state or when state changes are complex.

---

## Core Concepts

- **Store** → Holds the application state.
- **State** → Current data stored in Redux.
- **Action** → Describes what happened.
- **Reducer** → Decides how the state should change.
- **Dispatch** → Sends an action.
- **Selector** → Reads data from the store.

---

## Redux Data Flow

    UI
     ↓
    dispatch(action)
     ↓
    Reducer
     ↓
    New State
     ↓
    Store
     ↓
    Selector
     ↓
    UI updates

Redux follows a **one-way data flow**.

---

## Action

An action is usually an object with a `type` and optional `payload`.

    const action = {
      type: "counter/incremented",
      payload: 1
    };

### Remember

    Action → What happened?

---

## Reducer

A reducer is a function that receives the current state and action and returns the new state.

    function counterReducer(state = { value: 0 }, action) {
      switch (action.type) {
        case "counter/incremented":
          return {
            ...state,
            value: state.value + 1
          };

        default:
          return state;
      }
    }

Reducers should be **pure** and should not perform API calls, random operations, or other side effects.

### Remember

    Reducer → How should state change?

---

## Dispatch

`dispatch()` sends an action to Redux.

    dispatch({
      type: "counter/incremented"
    });

### Remember

    Dispatch → Send the action

---

## Selector

A selector reads data from the Redux store.

    const count = useSelector(
      (state) => state.counter.value
    );

### Remember

    Selector → Read state

---

## Store

The store holds the application's Redux state.

Modern Redux applications usually create the store using **Redux Toolkit's `configureStore`**.

---

## Interview Answer

> Redux is a state-management library for shared client state. Components dispatch actions, reducers calculate the new state, the store holds that state, and selectors read the required data.