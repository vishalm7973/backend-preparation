# JavaScript Scope, Hoisting, and Closures

## Execution context and call stack

When JavaScript runs a function, it creates an execution context containing its local bindings and execution state. Calls are placed on the **call stack**; a returned function's context is removed. A very deep unbounded recursion can overflow the stack.

## Lexical scope and scope chain

**Lexical scope** means a function's access to variables is determined by where the function is written, not where it is called. When a name is not found locally, JavaScript searches outward through enclosing scopes; this is the **scope chain**.

```javascript
const outerValue = "outer";

function makeReader() {
  const innerValue = "inner";
  return function read() {
    return `${outerValue}-${innerValue}`;
  };
}

const read = makeReader();
read(); // "outer-inner"
```

`var` is function-scoped. `let` and `const` are block-scoped, so bindings declared inside `{ ... }` do not escape that block.

## Hoisting and the temporal dead zone

Declarations are processed before normal statements execute, but not all declarations behave the same way:

- A function declaration can generally be called before its textual position.
- A `var` declaration is hoisted and initialized to `undefined`.
- `let` and `const` bindings exist from the start of their scope but cannot be accessed before their declaration is evaluated. That interval is the **temporal dead zone (TDZ)**.
- Function expressions and arrow functions assigned to `let`/`const` bindings follow the binding's TDZ rules.

```javascript
console.log(value); // undefined
var value = 3;

// console.log(rate); // ReferenceError (TDZ)
let rate = 2;
```

## Closures

A **closure** is a function together with access to variables from its lexical environment. The function can continue using those bindings after the outer function has returned.

```javascript
function createCounter() {
  let count = 0;
  return function increment() {
    count += 1;
    return count;
  };
}

const next = createCounter();
next(); // 1
next(); // 2
```

Closures are useful for private state, callbacks, factories, and currying. They also retain references: a long-lived closure can keep otherwise-unused data in memory.

## Classic loop closure interview question

`var` creates one function-scoped binding shared by loop iterations; `let` creates a per-iteration binding in a `for` loop.

```javascript
const callbacks = [];
for (let index = 0; index < 3; index += 1) {
  callbacks.push(() => index);
}
callbacks.map((callback) => callback()); // [0, 1, 2]
```

Interview prompts: explain lexical scope vs. dynamic scope, the scope chain, what is hoisted, why TDZ exists, and how closures retain state.
