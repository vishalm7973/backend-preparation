# JavaScript Functions, `this`, and Currying

## 1. Function Forms

There are three common ways to create functions:

    // Function Declaration
    function add(a, b) {
      return a + b;
    }

    // Function Expression
    const add = function (a, b) {
      return a + b;
    };

    // Arrow Function
    const add = (a, b) => a + b;

### Remember

    Function declaration → Hoisted
    Function expression  → Stored in a variable
    Arrow function       → Shorter syntax, no own `this`

---

## 2. First-Class & Higher-Order Functions

### First-Class Functions

Functions are treated like values.

They can be:

- Stored in variables
- Passed as arguments
- Returned from functions

    const greet = () => "Hello";

    function execute(fn) {
      return fn();
    }

    execute(greet); // "Hello"

### Higher-Order Function

A function that **accepts another function or returns a function**.

    function applyTo(value, transform) {
      return transform(value);
    }

    applyTo(3, number => number * 2);

    // 6

`map()`, `filter()`, and `reduce()` are common examples.

---

## 3. Callback

A **callback** is a function passed to another function to be executed later or at a specific point.

    function greet(name, callback) {
      console.log("Hello " + name);
      callback();
    }

    greet("John", () => {
      console.log("Done");
    });

    // Hello John
    // Done

### Remember

    Callback → Function passed to another function

---

## 4. Understanding `this`

For a normal function, `this` depends on **how the function is called**.

### Object Method

    const user = {
      name: "John",

      greet() {
        console.log(this.name);
      }
    };

    user.greet();

    // John

Here, `this` refers to `user`.

### `call()`

Calls the function immediately and sets `this`.

    function greet() {
      console.log(this.name);
    }

    const user = { name: "John" };

    greet.call(user);

    // John

### `apply()`

Same as `call()`, but arguments are passed as an array.

    function add(a, b) {
      return this.value + a + b;
    }

    const obj = { value: 10 };

    add.apply(obj, [2, 3]);

    // 15

### `bind()`

Returns a **new function** with `this` fixed.

    function greet() {
      console.log(this.name);
    }

    const user = { name: "John" };

    const greetUser = greet.bind(user);

    greetUser();

    // John

### Remember

    call()  → Calls immediately, arguments separately
    apply() → Calls immediately, arguments as array
    bind()  → Returns a new function

---

## 5. Arrow Functions & `this`

Arrow functions do **not have their own `this`**.

They use `this` from their surrounding scope.

    const user = {
      name: "John",

      greet() {
        const sayHello = () => {
          console.log(this.name);
        };

        sayHello();
      }
    };

    user.greet();

    // John

### Remember

    Normal function → `this` depends on how it is called
    Arrow function  → `this` comes from surrounding scope

---

## 6. Currying

**Currying** converts a function with multiple arguments into a chain of functions.

    const add = (a, b) => a + b;

    const curriedAdd = (a) => (b) => a + b;

    console.log(curriedAdd(2)(3));

    // 5

### Easy Example

    const multiply = (a) => (b) => a * b;

    multiply(2)(5);

    // 10

Currying is useful when you want to create reusable functions.

---

## 7. Partial Application

**Partial application** means fixing some arguments in advance.

    const add = (a, b) => a + b;

    const addTen = add.bind(null, 10);

    console.log(addTen(5));

    // 15

Here, `10` is already fixed.

### Currying vs Partial Application

    Currying
    → One argument at a time

    add(2)(3)

    Partial application
    → Fix some arguments beforehand

    add.bind(null, 10)