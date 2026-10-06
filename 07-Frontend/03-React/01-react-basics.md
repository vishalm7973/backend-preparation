# React Basics

## What Is React?

React is a JavaScript library for building user interfaces from reusable components. You describe the UI for the current props and state; React updates the rendered interface when those inputs change.

## Why React?

- Breaks interfaces into reusable, composable components.
- Provides a declarative model for describing UI state.
- Supports local and shared state patterns, a broad ecosystem, and multiple rendering strategies.
- Does not prescribe routing, data fetching, or every application architecture choice; these are often supplied by libraries or frameworks.

## What Is Vite, and Why Use It with React?

**React** is the UI library. **Vite** is a development server and build tool that can scaffold and run a React project. It serves the app during development with fast updates (hot module replacement) and builds optimized static assets for production.

React does not require Vite. A small demo can load React from a CDN without a project build setup, and production apps can use other tools or frameworks. However, JSX and many modern JavaScript features need transformation for broad browser support, while real apps also need dependency handling, asset processing, and a production build. Vite provides these common pieces with little setup; it is not a React replacement and does not itself add routing, state management, or a backend.

```bash
npm create vite@latest my-react-app -- --template react
cd my-react-app
npm install
npm run dev
```

Use the `react-ts` template for a TypeScript starter. For a client-rendered React app, Vite is a common lightweight choice; for routing, server rendering, or full-stack features, a React framework may be more suitable. Create React App is deprecated for new projects, so prefer a maintained build tool or framework.

## React vs. Vanilla JavaScript

With vanilla JavaScript, application code commonly queries and mutates DOM nodes directly. In React, application code updates state and describes the desired UI; React reconciles the component output and commits necessary DOM changes.

```javascript
// Vanilla JavaScript: imperative DOM update
document.querySelector("#count").textContent = String(count);

// React: declarative UI
function Counter({ count }) {
  return <p>{count}</p>;
}
```

React still uses the browser DOM. It manages updates; it does not replace the DOM.

## Components and Functional Components

A component is a reusable unit of UI. Modern React primarily uses function components. Component names start with a capital letter, and rendering should be pure: the same props, state, and context should produce the same UI.

```jsx
function Greeting({ name }) {
  return <h1>Hello, {name}</h1>;
}
```

## JSX

JSX is syntax that lets JavaScript code describe a UI tree. It is transformed by the build tool into React element creation calls. Use `{}` to embed JavaScript expressions; JSX uses `className` for CSS classes and requires one parent element or a fragment.

```jsx
function Status({ online }) {
  return (
    <>
      <p className={online ? "online" : "offline"}>
        {online ? "Online" : "Offline"}
      </p>
    </>
  );
}
```

JSX expressions are not arbitrary statements: use expressions such as conditional expressions or `array.map`, not a `for` statement directly inside braces.

## Props

Props are read-only inputs passed from a parent to a child. A child should not mutate its props; it can request a change by calling a callback supplied by its parent.

```jsx
function Product({ title, price }) {
  return <p>{title}: ${price}</p>;
}
<Product title="Keyboard" price={80} />
```

## State

State is data owned by a component that can change over time. Updating state schedules a render; it does not mutate the current render's state snapshot. Use the setter returned by `useState` rather than assigning to a state variable.

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount((current) => current + 1)}>{count}</button>;
}
```

Use the functional updater when the next state depends on the previous state. Treat objects and arrays in state immutably by creating updated copies.

## Props vs. State

| Props | State |
|---|---|
| Supplied by a parent | Owned by the component or its state owner |
| Read-only to the receiving component | Updated through a state setter or reducer |
| Communicate data/configuration downward | Represents data that changes over time |

Avoid storing a value in state if it can be derived from existing props or state; duplicated state can get out of sync.

## Component Composition

Composition builds larger interfaces by combining smaller components. A parent can pass content through the special `children` prop or pass a component as a prop.

```jsx
function Panel({ title, children }) {
  return (
    <section>
      <h2>{title}</h2>
      {children}
    </section>
  );
}

<Panel title="Account"><AccountDetails /></Panel>
```

Prefer composition for reusable layouts before adding complex inheritance-like component APIs.

## Interview reminders

Explain declarative rendering, one-way data flow, props vs. state, why components are composed, and how React differs from manually updating the DOM.
