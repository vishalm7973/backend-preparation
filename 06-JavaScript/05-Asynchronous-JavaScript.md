# Asynchronous JavaScript: Callbacks, Promises, and the Event Loop

JavaScript executes synchronous code on a call stack. Asynchronous APIs arrange for callbacks or Promise reactions to run later; they do not make a long synchronous function non-blocking.

## Callbacks and callback hell

A **callback** is a function passed to another function to run after an operation or event. Deeply nested callbacks can make flow, error handling, and sequencing difficult; this is commonly called **callback hell**.

```javascript
getUser(userId, (userError, user) => {
  if (userError) return handleError(userError);
  getOrders(user.id, (orderError, orders) => {
    if (orderError) return handleError(orderError);
    getShipping(orders[0].id, (shippingError, shipping) => {
      if (shippingError) return handleError(shippingError);
      show(shipping);
    });
  });
});
```

Use named functions, return early on errors, or compose operations with Promises and `async`/`await` to make control flow easier to follow.

## Promises

A Promise represents a result that may be available later. It is pending, fulfilled with a value, or rejected with a reason. A settled Promise does not change state again.

```javascript
fetchUser(userId)
  .then((user) => fetchOrders(user.id))
  .then((orders) => show(orders))
  .catch((error) => handleError(error))
  .finally(() => hideSpinner());
```

- `.then(onFulfilled, onRejected)` registers reactions and returns a new Promise.
- Return a value from `.then()` to pass it down the chain; throw or return a rejected Promise to pass failure down.
- `.catch(handler)` handles rejection and itself returns a Promise, so a thrown error in the handler can reject the next link.
- `.finally(handler)` runs after settlement for cleanup and normally preserves the prior value or rejection.
- Avoid forgetting to `return` an inner Promise; otherwise the outer chain will not wait for it.

## Promise combinators

| Method | Behavior |
|---|---|
| `Promise.all(iterable)` | Fulfills when all fulfill, preserving input order; rejects when one rejects. |
| `Promise.allSettled(iterable)` | Waits for every input and reports each fulfillment/rejection. |
| `Promise.race(iterable)` | Settles with the first input to settle. |
| `Promise.any(iterable)` | Fulfills with the first fulfillment; rejects with `AggregateError` if all reject. |

```javascript
const [profile, orders] = await Promise.all([
  fetchProfile(userId),
  fetchOrders(userId)
]);
```

Use `Promise.all` when all results are required and tasks are independent. It does not cancel remaining operations when one rejects.

## async / await

An `async` function always returns a Promise. `await` pauses that function until the Promise settles; it does not block the JavaScript thread.

```javascript
async function loadOrders(userId) {
  try {
    const orders = await fetchOrders(userId);
    return orders;
  } catch (error) {
    throw new Error("Could not load orders", { cause: error });
  }
}
```

Sequential `await` is appropriate for dependent operations. For independent work, start both and await them together with `Promise.all` to avoid unnecessary sequential latency.

## Event loop, tasks, and microtasks

The event loop runs queued work after the current synchronous stack completes. Promise reactions and `queueMicrotask` use the microtask queue; timers and many event callbacks use task queues. After the current task, queued microtasks are generally drained before the next task.

```javascript
console.log("start");
setTimeout(() => console.log("timer"), 0);
Promise.resolve().then(() => console.log("promise"));
console.log("end");
// start, end, promise, timer
```

The exact event-loop phases and I/O behavior differ between browser JavaScript and Node.js. See the existing [Node.js event-loop notes](../01-Backend/03-Node.js/01-event-loop.md) for Node-specific phases.

## Interview reminders

Explain callback error conventions, Promise chaining and error propagation, `async`/`await`, the difference between concurrency and parallel execution, why independent operations can use `Promise.all`, and the task-vs-microtask ordering in output questions.
