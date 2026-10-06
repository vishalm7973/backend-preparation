# Asynchronous JavaScript: Callbacks, Promises, and Event Loop

## 1. Callbacks & Callback Hell

A **callback** is a function passed to another function to run later.

    function greet(name, callback) {
      console.log("Hello " + name);
      callback();
    }

    greet("John", () => {
      console.log("Done");
    });

### Callback Hell

When callbacks are deeply nested, code becomes difficult to read and handle errors.

    getUser(id, (user) => {
      getOrders(user.id, (orders) => {
        getPayment(orders[0].id, (payment) => {
          console.log(payment);
        });
      });
    });

### Remember

    Callback → Function passed to another function

    Callback Hell → Too many nested callbacks

Promises and `async/await` make this easier to manage.


## 2. Promises

A **Promise** represents a result that will be available in the future.

A Promise has 3 states:

    pending
    fulfilled
    rejected

### Example

    fetchUser()
      .then((user) => {
        return fetchOrders(user.id);
      })
      .then((orders) => {
        console.log(orders);
      })
      .catch((error) => {
        console.log(error);
      })
      .finally(() => {
        console.log("Done");
      });

### Remember

    then()    → Success
    catch()   → Error
    finally() → Runs after success or failure

Each `.then()` returns a **new Promise**, so we can chain them.


## 3. Promise Error Handling

Errors can move down the Promise chain.

    fetchUser()
      .then((user) => {
        return fetchOrders(user.id);
      })
      .catch((error) => {
        console.log(error);
      });

### Important

Always `return` an inner Promise when chaining.

    .then(() => {
      return fetchOrders();
    });

Without `return`, the next `.then()` may not wait for it.

### Error Handling

Use `try...catch`.

    async function getData() {
      try {
        const data = await fetchData();
        return data;
      } catch (error) {
        console.log(error);
      }
    }


# 6. Event Loop

JavaScript runs synchronous code on the **call stack**.

Asynchronous work is handled by the runtime and its callbacks/Promise reactions are queued for later.

### Important Queues

    Task queue
    → setTimeout, events, etc.

    Microtask queue
    → Promises, queueMicrotask

After the current synchronous code finishes, **microtasks are generally processed before the next task**.

### Example

    console.log("start");

    setTimeout(() => {
      console.log("timer");
    }, 0);

    Promise.resolve().then(() => {
      console.log("promise");
    });

    console.log("end");

Output:

    start
    end
    promise
    timer

### Why?

1. Synchronous code runs first.
2. Promise callback goes to the microtask queue.
3. Timer callback goes to a task queue.
4. Microtasks are processed before the next task.