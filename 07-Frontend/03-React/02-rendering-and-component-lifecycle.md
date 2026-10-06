# React Rendering and Component Lifecycle

## 1. How React Renders

When props, state, or context change:

    Props / State / Context
            ↓
        Render
            ↓
       Reconcile
            ↓
       Commit DOM changes

### Remember

**Render** → Calculate what UI should look like  
**Reconcile** → Compare old vs new UI  
**Commit** → Apply required DOM changes

React does **not** rebuild the entire DOM on every render.

---

## 2. Re-rendering

A component can re-render when:

- Its state changes
- Its parent re-renders
- Its props change
- A context it uses changes
- A subscribed external store changes

A re-render does **not always mean the DOM changes**.

React may find that the UI is already the same.

---

## 3. Mount, Update, Unmount

### Mount

Component is added to the UI.

### Update

Component renders again because its props, state, context, etc. changed.

### Unmount

Component is removed from the UI.

Cleanup is important for:

- Timers
- Event listeners
- Subscriptions
- Other external resources

---

## 4. `useEffect` Lifecycle

An effect runs **after the render is committed**.

    useEffect(() => {
      console.log("Effect runs");

      return () => {
        console.log("Cleanup");
      };
    }, []);

Cleanup runs when the component unmounts.

With dependencies:

    useEffect(() => {
      console.log("Effect runs");

      return () => {
        console.log("Cleanup");
      };
    }, [userId]);

Here:

    userId changes
        ↓
    Cleanup previous effect
        ↓
    Run new effect

---

## 5. Virtual DOM & Diffing

**Virtual DOM** is a common term for React's in-memory representation of the UI.

React compares the previous UI with the new UI.

This comparison is often called **diffing**.

    Old UI
      ↓
    Compare
      ↑
    New UI

React then updates only the necessary DOM parts.

---

## 6. Reconciliation

**Reconciliation** is the process of comparing the old and new element trees and deciding what needs to change.

If React can preserve the component's identity, its state can be preserved.

Changing a component's `key` can intentionally create a new identity and reset its state.

---

## 7. Keys

Keys help React identify items in a list.

    {users.map((user) => (
      <UserRow
        key={user.id}
        user={user}
      />
    ))}

### Good

Use a stable unique ID:

    key={user.id}

### Avoid

Using array index when the list can change:

    key={index}

Also avoid random keys:

    key={Math.random()}

A new key makes React treat the item as a new component.

### Important

`key` is special React metadata. It is **not available as a normal prop**.