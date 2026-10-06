# Node.js Internals

## 1. V8 Engine

**V8** is Google's JavaScript engine. Node.js uses V8 to run JavaScript.

Node.js = **Runtime**
V8 = **Runs JavaScript**
libuv = **Async I/O + Event Loop**

### V8 handles:
- Parsing JavaScript
- Executing JavaScript
- JIT optimization
- Memory management
- Garbage collection
- Objects, functions, closures

**Remember:** V8 runs JS; Node.js provides the runtime around it.

---

## 2. libuv

**libuv** is a C library used by Node.js for asynchronous operations.

It provides:
- Event Loop
- Async I/O
- Thread Pool

It helps Node.js handle I/O without blocking the main JavaScript thread.

---

## 3. libuv Thread Pool

Some operations use libuv's **Thread Pool** instead of the main JS thread.

Examples:
- File system operations
- Some DNS operations
- Cryptography
- Compression

Default size: **4 threads**

Can be changed:

    UV_THREADPOOL_SIZE=8 node server.js

**Important:** Not every async operation uses the Thread Pool. Some are handled directly by the OS.

---

## 4. Event Loop Blocking

**Event Loop blocking** means the main JavaScript thread is busy with synchronous work and cannot process other callbacks.

Example:

    while (true) {
      // CPU-heavy work
    }

While this runs, other requests can be delayed.

For CPU-heavy work, consider:
- Worker Threads
- Background workers
- Child processes
- Separate services

---

## Why is Node.js good for I/O-heavy applications?

Node.js uses **non-blocking I/O + Event Loop**.

For example:

    DB Query starts
         ↓
    Node.js doesn't wait
         ↓
    Handles other requests
         ↓
    DB result comes back
         ↓
    Node.js processes the result

### Interview Answer

> Node.js is well suited for I/O-heavy applications because it uses non-blocking asynchronous I/O and an Event Loop. While waiting for a database or API response, Node.js can handle other requests instead of blocking the main thread.