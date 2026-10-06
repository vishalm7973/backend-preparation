# JavaScript Scope, Hoisting, and Closures

## 1. Execution Context & Call Stack

When JavaScript runs a function, it creates an **execution context**.

Function calls are stored in the **call stack**.

    function greet() {
      console.log("Hello");
    }

    greet();

    // greet() is added to the call stack
    // function finishes → removed from the stack

Too much recursion can cause **stack overflow**.

---

## 2. Lexical Scope & Scope Chain

**Lexical scope** means a function can access variables based on **where it is written**, not where it is called.

If JavaScript cannot find a variable locally, it searches the outer scope. This is the **scope chain**.

    const outer = "outer";

    function parent() {
      const inner = "inner";

      function child() {
        console.log(outer);
        console.log(inner);
      }

      child();
    }

    parent();

    // outer
    // inner

### Remember

    Lexical scope → Where the function is written
    Scope chain   → Search from inner scope → outer scope

`var` → Function-scoped

`let` / `const` → Block-scoped

    {
      let x = 10;
      const y = 20;
    }

    console.log(x); // Error
    console.log(y); // Error

---

## 3. Hoisting & TDZ

**Hoisting** means JavaScript makes declarations available before the code is executed.

But different declarations behave differently.

    var       → Hoisted as undefined
    let/const → Hoisted but in TDZ
    function  → Fully hoisted

**TDZ** is the time between entering a scope and reaching the `let` or `const` declaration.

During this time, you cannot access the variable.

    console.log(age); // ReferenceError

    let age = 25;

### `var`

`var` is hoisted and initialized with `undefined`.

    console.log(value);

    var value = 3;

    // undefined

### `let` / `const`

They are hoisted but **cannot be accessed before declaration**.

This period is called the **Temporal Dead Zone (TDZ)**.

    console.log(age); // ReferenceError

    let age = 25;

### Function Declaration

Function declarations can generally be called before they are written.

    greet();

    function greet() {
      console.log("Hello");
    }

    // Hello

 ### Function Expression

A function expression is assigned to a variable.

    greet();

    var greet = function () {
      console.log("Hello");
    };

    // undefined is not a function
    // TypeError

Why?

`var greet` is hoisted, but only the variable declaration is hoisted.

    var greet; // undefined

    greet();   // TypeError

    greet = function () {
      console.log("Hello");
    };

### Remember

    var        → Hoisted + initialized as undefined
    let/const  → Hoisted + TDZ
    function   → Can be called before declaration

---

## 4. Closures

A **closure** is a function that remembers variables from its outer scope.

The function can still access those variables even after the outer function has finished.

    function createCounter() {
      let count = 0;

      return function increment() {
        count++;
        return count;
      };
    }

    const counter = createCounter();

    console.log(counter()); // 1
    console.log(counter()); // 2
    console.log(counter()); // 3

`count` is remembered by the inner function.

### Common Uses

- Private state
- Callbacks
- Function factories
- Currying

### Remember

> Closure = Function + access to its outer variables

---

## 5. Loop Closure Interview Question

With `let`, each loop iteration gets its own binding.

    const callbacks = [];

    for (let index = 0; index < 3; index++) {
      callbacks.push(() => index);
    }

    console.log(callbacks.map(callback => callback()));

    // [0, 1, 2]

With `var`, there is one shared function-scoped variable.

    const callbacks = [];

    for (var index = 0; index < 3; index++) {
      callbacks.push(() => index);
    }

    console.log(callbacks.map(callback => callback()));

    // [3, 3, 3]

### Remember

    let → New binding for each iteration
    var → Same shared binding