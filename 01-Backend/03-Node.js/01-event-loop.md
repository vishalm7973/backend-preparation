# Node.js Event Loop

## 1. What is the Event Loop?

The **Event Loop** allows Node.js to handle asynchronous operations without blocking the main JavaScript thread.

Example:

    Request A → Waiting for DB
    Request B → Processing
    Request A → DB completed → Continue

This allows Node.js to handle many I/O operations concurrently.

---

## 2. Why do we need the Event Loop?

Node.js executes JavaScript mainly on **one main thread**.

The Event Loop allows that thread to:

- Handle multiple requests
- Wait for I/O without blocking
- Run callbacks when operations finish

**Remember:**

> Event Loop = Keeps JavaScript execution moving while async work is in progress.

---

# 3. Event Loop Phases

Simplified order:

    Timers
       ↓
    Pending Callbacks
       ↓
    Poll
       ↓
    Check
       ↓
    Close Callbacks

### Timers

Handles:

    setTimeout()
    setInterval()

The delay is a **minimum threshold**, not an exact execution time.

### Pending Callbacks

Handles some system-level I/O callbacks that were deferred.

### Poll

Handles most I/O-related callbacks.

Examples:

- File I/O
- Network I/O

### Check

Handles:

    setImmediate()

### Close Callbacks

Handles close-related callbacks.

Example:

    socket.on("close", ...)

---

# 4. Microtasks

Node.js has two important microtask queues:

    process.nextTick()
    Promise / queueMicrotask()

`process.nextTick()` has **special priority**.

After a callback finishes, Node.js processes:

    nextTick
       ↓
    Promise / queueMicrotask
       ↓
    Continue Event Loop

Example:

    console.log("1");

    Promise.resolve().then(() => console.log("4"));

    process.nextTick(() => console.log("6"));

    console.log("7");

Output:

    1
    7
    6
    4

---

# 5. Event Loop Inside an I/O Callback

Example:

    fs.readFile(__filename, () => {
      console.log("2");

      setTimeout(() => console.log("3"), 0);
      setImmediate(() => console.log("4"));

      process.nextTick(() => console.log("5"));
      Promise.resolve().then(() => console.log("6"));
    });

Inside the I/O callback:

    2
    ↓
    5  (nextTick)
    ↓
    6  (Promise)
    ↓
    4  (setImmediate)
    ↓
    3  (setTimeout)

---

# 6. CPU-Heavy Work

Synchronous CPU-heavy JavaScript **blocks the main thread**.

Example:

    for (let i = 0; i < 10000000000; i++) {
      // Heavy calculation
    }

While this runs:

    Event Loop → BLOCKED

For CPU-heavy work, consider:

- Worker Threads
- Background workers
- Child processes
- Separate services

---

# 7. Is Node.js Single-Threaded?

**Yes, mainly for JavaScript execution.**

Node.js has:

- One main JavaScript thread
- Event Loop
- libuv
- Thread Pool
- Worker Threads when needed

So the accurate answer is:

> Node.js executes JavaScript primarily on a single main thread, but the overall runtime can use additional threads and processes.

---

# 8. Event Loop vs Thread Pool

### Event Loop

Runs JavaScript callbacks on the **main thread**.

### Thread Pool

Uses worker threads for certain operations handled by **libuv**.

Examples:

- Some file-system operations
- Some DNS operations
- Certain CPU-heavy native operations

**Remember:**

    Event Loop → Executes callbacks

    Thread Pool → Performs certain background operations

They work together but are **not the same thing**.

---

# 9. Does setTimeout(fn, 0) Run Immediately?

**No.**

`0` means the timer becomes eligible after the minimum delay.

It still depends on:

- Event Loop
- Current workload
- Whether the JavaScript thread is available

---

# 10. Does CPU-Heavy Code Block Node.js?

**Yes**, if it is synchronous JavaScript.

The code runs on the main JavaScript thread, so the Event Loop cannot process other JavaScript callbacks while it is running.