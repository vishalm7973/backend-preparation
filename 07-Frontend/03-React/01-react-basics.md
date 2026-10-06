# React Basics

## 1. What Is React?

React is a **JavaScript library for building user interfaces** using reusable components.

You describe **what the UI should look like**, and React updates it when state or props change.

---

## 2. Why React?

- Reusable components
- Declarative UI
- Easy state management
- Component composition
- Large ecosystem

React itself does not provide everything like routing or data fetching. Other libraries/frameworks can be used for these.

---

## 3. React vs Vite

**React** → UI library

**Vite** → Development server and build tool

Vite helps you create and run a React project with fast development updates.

    npm create vite@latest my-app -- --template react
    cd my-app
    npm install
    npm run dev

For TypeScript:

    npm create vite@latest my-app -- --template react-ts

### Remember

    React → Builds UI
    Vite  → Runs and builds the project

Vite is **not a replacement for React**.

---

## 4. React vs Vanilla JavaScript

### Vanilla JavaScript

You directly modify the DOM.

    document.querySelector("#count").textContent = count;

### React

You update state and describe the UI.

    function Counter({ count }) {
      return <p>{count}</p>;
    }

### Remember

    Vanilla JS → Direct DOM manipulation
    React      → Declarative UI + React manages updates

React still uses the browser DOM.

---

## 5. Components

A component is a **reusable piece of UI**.

Modern React mainly uses function components.

    function Greeting({ name }) {
      return <h1>Hello, {name}</h1>;
    }

Use it:

    <Greeting name="John" />

Component names should start with a **capital letter**.

---

## 6. JSX

JSX allows us to write UI-like syntax inside JavaScript.

    function Status({ online }) {
      return (
        <p className={online ? "online" : "offline"}>
          {online ? "Online" : "Offline"}
        </p>
      );
    }

Use `{}` to put JavaScript expressions inside JSX.

    <h1>{name}</h1>

Use `className` instead of `class`.

A component should return one parent element or a fragment:

    <>
      <h1>Hello</h1>
      <p>Welcome</p>
    </>

---

## 7. Props

**Props** are data passed from a parent component to a child.

Props are **read-only**.

    function Product({ title, price }) {
      return <p>{title}: ${price}</p>;
    }

    <Product title="Keyboard" price={80} />

### Remember

    Parent
      ↓
    Props
      ↓
    Child

The child should not modify its props.

---

## 8. State

**State** is data that can change over time.

Use `useState()` to create state.

    import { useState } from "react";

    function Counter() {
      const [count, setCount] = useState(0);

      return (
        <button onClick={() => setCount(count + 1)}>
          {count}
        </button>
      );
    }

When state changes, React schedules a **re-render**.

Do not directly modify state.

    count = count + 1; // ❌

Use the setter:

    setCount(count + 1); // ✅

If the new value depends on the previous value, use a functional updater:

    setCount((current) => current + 1);

---

## 9. Props vs State

| Props | State |
|---|---|
| Comes from parent | Owned by component/state owner |
| Read-only | Can be updated |
| Used to pass data | Used for changing data |
| Parent → Child | Changes trigger re-render |

### Easy Remember

    Props → Data coming in
    State → Data that changes

Avoid storing data in state if it can be calculated from existing props/state.

---

## 10. Component Composition

Composition means **building bigger components using smaller components**.

### `children`

    function Panel({ title, children }) {
      return (
        <section>
          <h2>{title}</h2>
          {children}
        </section>
      );
    }

    <Panel title="Account">
      <AccountDetails />
    </Panel>

Here, `AccountDetails` is passed through `children`.

### Remember

> Prefer composition to make components reusable and flexible.

---

## 11. `children` Prop

`children` is a special React prop that contains the content placed **between a component's opening and closing tags**.

    function Panel({ children }) {
      return (
        <div>
          {children}
        </div>
      );
    }

    <Panel>
      <h1>Hello</h1>
      <p>Welcome!</p>
    </Panel>

Here, everything inside `<Panel>` becomes the `children` prop.

### Why use it?

It makes components **reusable and flexible**.

    function Card({ children }) {
      return (
        <div className="card">
          {children}
        </div>
      );
    }

    <Card>
      <h2>Profile</h2>
      <p>John</p>
    </Card>

The `Card` component does not need to know what content will be inside it.

### Remember

    children → Content passed between component tags