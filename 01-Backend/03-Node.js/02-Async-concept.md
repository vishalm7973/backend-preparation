# Async Programming in Node.js

## 1. Callback

A **callback** is a function passed to another function and called later when an async operation finishes.

**Problem:** Too many nested callbacks → **Callback Hell**.

---

## 2. Promise

A **Promise** represents the future result of an async operation.

### States
- **Pending** → still running
- **Fulfilled** → completed successfully
- **Rejected** → failed

### Promise Chaining
`.then()` can be chained. Each `.then()` waits for the Promise returned by the previous one.

### `.catch()`
Handles errors.

- `catch()` returns a value → chain becomes fulfilled → next `.then()` runs.
- `catch()` throws/rejects → chain remains rejected → next `.then()` is skipped.

### Promise APIs

| API | Behavior |
|---|---|
| `Promise.all()` | All must succeed |
| `Promise.allSettled()` | Wait for all, success or failure |
| `Promise.race()` | First Promise to settle |
| `Promise.any()` | First Promise to succeed |

### When to use
- `Promise.all()` → need all independent results.
- `Promise.allSettled()` → want results even if some fail.
- `Promise.race()` → timeout or first result.
- `Promise.any()` → first successful result.

---

## 3. async/await

`async/await` is a cleaner way to work with Promises.

- `async` function always returns a Promise.
- `await` pauses the current async function.
- `await` **does NOT block the Event Loop**.
- Use `try/catch` for error handling.

### Interview
**Does await block Node.js?**

> No. `await` pauses only the current async function. The Event Loop can continue handling other work.

---

## 4. Sequential vs Concurrent

### Sequential
One operation waits for the previous one.

Use when operations **depend on each other**.

    const user = await getUser();
    const orders = await getOrders(user.id);

### Concurrent
Independent operations start together.

    const [users, products] = await Promise.all([
      getUsers(),
      getProducts()
    ]);

**Remember:**

    Sequential: 2s + 3s = 5s
    Concurrent: max(2s, 3s) = 3s

---

## 5. Error Propagation

An error moves from where it happened to the calling function until it is handled.

With `async/await`, a rejected Promise causes `await` to throw.

Use:

    try {
      await someOperation();
    } catch (error) {
      // handle error
    }

---

## 6. Cancellation

**Cancellation** means stopping an async operation when it is no longer needed.

Examples:
- User cancels request
- Client disconnects
- Application shuts down

Node.js provides:

`AbortController` + `AbortSignal`

### Cancellation vs Timeout

- **Cancellation** → operation is no longer needed.
- **Timeout** → operation took too long.

---

## 7. Concurrency vs Parallelism

**Concurrency:** Multiple tasks make progress during the same period.

**Parallelism:** Multiple tasks actually execute at the same time, usually using multiple CPU cores/threads.

### Why concurrency matters in Node.js

While Node.js waits for I/O like a database query, it can handle other requests and tasks.

---

## 8. Concurrency Limits

Don't start thousands of operations at once.

Example:

    10,000 operations
         ↓
    Allow only 10 at a time

Why?
- API rate limits
- DB connection exhaustion
- High memory usage
- Too many connections

**Concurrency limit = maximum number of async operations running at the same time.**