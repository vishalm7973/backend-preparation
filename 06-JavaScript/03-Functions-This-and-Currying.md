# JavaScript Functions, `this`, and Currying

## Function forms

```javascript
function declaration(value) { return value; }
const expression = function (value) { return value; };
const arrow = (value) => value;
```

Function declarations are hoisted with their body. Function expressions and arrow functions are values assigned to bindings and follow the binding's initialization rules. Arrow functions do not have their own `this`, `arguments`, or constructor behavior.

## First-class and higher-order functions

Functions are values: they can be assigned, passed as arguments, and returned. A **higher-order function** accepts a function, returns one, or both. Array methods such as `map` and `filter` use callbacks.

```javascript
function applyTo(value, transform) {
  return transform(value);
}
applyTo(3, (number) => number * 2); // 6
```

A **callback** is a function passed to another function to run later or at a chosen point. Callback nesting can become hard to read; Promise chaining and `async`/`await` are common alternatives (see the async notes).

## Understanding `this`

For ordinary functions, `this` is primarily determined by how the function is called. In strict mode, a plain function call has `this === undefined`; a method call uses the receiver object. `call`, `apply`, and `bind` explicitly set the receiver. Arrow functions capture `this` lexically from their surrounding code.

```javascript
const account = {
  balance: 10,
  showBalance() { return this.balance; }
};
account.showBalance(); // 10

const detached = account.showBalance;
detached.call(account); // 10
```

- `fn.call(receiver, a, b)` invokes immediately with arguments listed individually.
- `fn.apply(receiver, [a, b])` invokes immediately with arguments in an array.
- `fn.bind(receiver, a)` returns a new function with `this` and optional leading arguments fixed.

## Currying and partial application

**Currying** transforms a function taking several arguments into a chain of single-argument functions. **Partial application** fixes some arguments ahead of time; it does not necessarily turn every remaining argument into a separate call.

```javascript
const add = (left, right) => left + right;
const curriedAdd = (left) => (right) => left + right;
curriedAdd(2)(3); // 5

const addTen = add.bind(null, 10); // partial application
addTen(5); // 15
```

Currying is useful for reusable configuration and function composition, but avoid it when it makes APIs harder to read.

## Interview reminders

Be ready to distinguish declarations from expressions, explain callback and higher-order function terminology, predict `this` for method/plain/call/bind/arrow cases, and demonstrate a closure-backed function or a curried function.
