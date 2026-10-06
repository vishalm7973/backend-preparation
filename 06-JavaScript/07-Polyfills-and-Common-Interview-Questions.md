# JavaScript Polyfills and Common Interview Questions

A **polyfill** supplies a missing API in an older runtime. Interview polyfills demonstrate language mechanics; production code should prefer native implementations and established compatibility libraries when possible. These examples use helper names instead of overwriting built-in prototypes.

## `map` polyfill

`map` calls the callback for each present array element and returns a new array of the same length. This version preserves sparse-array holes.

```javascript
function mapPolyfill(array, callback, thisArg) {
  if (array == null) throw new TypeError("array is null or undefined");
  if (typeof callback !== "function") throw new TypeError("callback must be a function");

  const source = Object(array);
  const length = source.length >>> 0;
  const result = new Array(length);

  for (let index = 0; index < length; index += 1) {
    if (index in source) {
      result[index] = callback.call(thisArg, source[index], index, source);
    }
  }
  return result;
}
```

This is an interview-sized implementation, not a complete specification polyfill for every array-like edge case.

## `reduce` polyfill

The initial value is optional. Without it, reduction starts at the first present element and throws for an empty array with no present elements.

```javascript
function reducePolyfill(array, callback, initialValue) {
  if (array == null) throw new TypeError("array is null or undefined");
  if (typeof callback !== "function") throw new TypeError("callback must be a function");

  const source = Object(array);
  const length = source.length >>> 0;
  const hasInitialValue = arguments.length >= 3;
  let index = 0;
  let accumulator = initialValue;

  if (!hasInitialValue) {
    while (index < length && !(index in source)) index += 1;
    if (index >= length) throw new TypeError("reduce of empty array with no initial value");
    accumulator = source[index];
    index += 1;
  }

  for (; index < length; index += 1) {
    if (index in source) {
      accumulator = callback(accumulator, source[index], index, source);
    }
  }
  return accumulator;
}
```

## `bind` polyfill (simplified)

`bind` returns a function with a chosen `this` value and optionally pre-filled leading arguments. This interview example handles normal calls; it does not implement constructor behavior or every native edge case.

```javascript
function bindPolyfill(fn, thisArg, ...boundArgs) {
  if (typeof fn !== "function") throw new TypeError("target must be a function");
  return function bound(...callArgs) {
    return fn.apply(thisArg, [...boundArgs, ...callArgs]);
  };
}
```

## `Promise.all` polyfill

Fulfills with results in input order when every input fulfills; rejects as soon as an input rejects. `Promise.resolve` accepts ordinary values as well as Promises/thenables.

```javascript
function promiseAllPolyfill(iterable) {
  return new Promise((resolve, reject) => {
    const items = Array.from(iterable);
    const results = new Array(items.length);
    let remaining = items.length;

    if (remaining === 0) {
      resolve(results);
      return;
    }

    items.forEach((item, index) => {
      Promise.resolve(item).then(
        (value) => {
          results[index] = value;
          remaining -= 1;
          if (remaining === 0) resolve(results);
        },
        reject
      );
    });
  });
}
```

## Common interview questions

### What is the difference between `map` and `forEach`?

`map` returns a new array of callback results. `forEach` returns `undefined` and is typically used for side effects.

### What is the difference between `slice` and `splice`?

`slice` returns a copy of a range without mutating the source. `splice` mutates the source by inserting/removing items and returns the removed items.

### What is a closure?

A function retaining access to its lexical environment after the outer function has returned. Closures enable private state and function factories.

### What is callback hell, and how do Promises help?

Deeply nested callbacks obscure sequencing and error handling. Promises compose asynchronous work through returned chains; `async`/`await` provides sequential-looking syntax over Promises.

### How does `this` differ in an arrow function?

An arrow function captures `this` from its lexical surrounding scope; it does not create its own `this` binding.

### What is the event-loop order in a simple example?

Synchronous statements run first; Promise microtasks normally run after the current task and before the next timer task. Node.js adds its own event-loop phases and APIs.

### Is JavaScript pass-by-reference?

JavaScript passes arguments by value. For objects, the value is a reference, so a function can mutate the referenced object but cannot reassign the caller's variable binding.

### What is the difference between `var`, `let`, and `const`?

`var` is function-scoped; `let` and `const` are block-scoped and have a TDZ before initialization. `const` prevents rebinding but does not make an object immutable.

For each answer, demonstrate with a small example and mention an edge case or tradeoff rather than relying only on a memorized definition.
