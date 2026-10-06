# JavaScript Polyfills and Common Interview Questions

## 1. What is a Polyfill?

A **polyfill** is code that provides a feature when the environment does not support it.

For interviews, we usually create a simplified version of methods like `map`, `reduce`, `bind`, or `Promise.all`.

---

## 2. `map()` Polyfill

`map()` creates a **new array** by transforming each element.

    function mapPolyfill(array, callback) {
      const result = [];

      for (let i = 0; i < array.length; i++) {
        result.push(callback(array[i], i, array));
      }

      return result;
    }

### Example

    const numbers = [1, 2, 3];

    const result = mapPolyfill(numbers, (n) => n * 2);

    console.log(result);
    // [2, 4, 6]

### Remember

    map → New array
    forEach → undefined


## 3. `reduce()` Polyfill

`reduce()` combines array values into **one result**.

    function reducePolyfill(array, callback, initialValue) {
      let result = initialValue;

      for (let i = 0; i < array.length; i++) {
        result = callback(result, array[i], i, array);
      }

      return result;
    }

### Example

    const numbers = [1, 2, 3];

    const total = reducePolyfill(
      numbers,
      (sum, n) => sum + n,
      0
    );

    console.log(total);
    // 6

### Important

If no initial value is provided, native `reduce()` starts with the first element.

---

## 4. `bind()` Polyfill

`bind()` returns a **new function** with `this` fixed.

    function bindPolyfill(fn, thisArg, ...boundArgs) {
      return function (...args) {
        return fn.apply(
          thisArg,
          [...boundArgs, ...args]
        );
      };
    }

### Example

    const user = {
      name: "John"
    };

    function greet(message) {
      console.log(message, this.name);
    }

    const greetUser = bindPolyfill(
      greet,
      user,
      "Hello"
    );

    greetUser();

    // Hello John

### Remember

    bind() → Returns a new function
    call() → Calls immediately
    apply() → Calls immediately


## 5. `Promise.all()` Polyfill

`Promise.all()` waits for **all Promises**.

If one rejects, the result rejects.

    function promiseAllPolyfill(promises) {
      return new Promise((resolve, reject) => {
        const results = [];
        let completed = 0;

        if (promises.length === 0) {
          resolve([]);
          return;
        }

        promises.forEach((promise, index) => {
          Promise.resolve(promise)
            .then((value) => {
              results[index] = value;
              completed++;

              if (completed === promises.length) {
                resolve(results);
              }
            })
            .catch(reject);
        });
      });
    }

### Example

    promiseAllPolyfill([
      Promise.resolve(1),
      Promise.resolve(2),
      Promise.resolve(3)
    ]).then(console.log);

    // [1, 2, 3]

### Remember

    Promise.all()
    → Waits for all
    → Keeps input order
    → Rejects if one rejects

---

## Is JavaScript Pass-by-Reference?

JavaScript is **pass-by-value**.

For objects, the value being passed is a **reference to the object**.

    function changeUser(user) {
      user.name = "Alex";
    }

    const user = {
      name: "John"
    };

    changeUser(user);

    console.log(user.name);

    // Alex

The object was modified because both references point to the same object.

But reassigning the parameter does not change the caller's variable:

    function changeUser(user) {
      user = {
        name: "Alex"
      };
    }

    const user = {
      name: "John"
    };

    changeUser(user);

    console.log(user.name);

    // John

