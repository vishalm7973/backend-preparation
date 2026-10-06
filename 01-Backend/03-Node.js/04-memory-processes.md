# Memory & Processes

## 1. Call Stack

The **Call Stack** keeps track of currently executing functions.

- Function called → added to stack
- Function finishes → removed
- Works in **LIFO** order: Last In → First Out

**Remember:**  
Stack → "What is executing now?"

---

## 2. Heap Memory

**Heap** stores dynamically created data like:

- Objects
- Arrays

**Remember:**  
Heap → "Where is dynamically allocated data stored?"

---

## 3. Garbage Collection

Garbage Collection automatically removes objects that are no longer needed.

This frees heap memory and helps prevent memory problems.

---

## 4. Memory Leak

A **memory leak** happens when unused objects are still referenced, so Garbage Collection cannot remove them.

Common causes:
- Unbounded caches
- Global references
- Event listeners
- Timers
- Growing arrays
- Closures

### How to find it

1. Check `process.memoryUsage()`
2. Monitor `heapUsed`
3. Take heap snapshots
4. Find unnecessary references
5. Fix and test again

---

## 5. Worker Threads

Worker Threads run JavaScript on **separate threads** in the same Node.js process.

Use them for **CPU-heavy work**:

- Image processing
- Video processing
- Large calculations
- Data transformation

**Remember:**

    CPU-bound → Worker Threads
    I/O-bound → Async I/O

Don't use Worker Threads just for HTTP requests or database queries.

---

## 6. child_process

`child_process` creates a **separate OS process**.

Use it to:
- Run shell commands
- Run Python scripts
- Run external programs
- Isolate work

Common methods:

- `spawn()` → stream input/output
- `exec()` → get complete output
- `fork()` → create another Node.js process

### Worker vs Child Process

    Worker Thread
    → Separate thread
    → Same Node.js process
    → Shared process resources

    Child Process
    → Separate OS process
    → Separate memory
    → Separate Event Loop

---

## 7. Cluster

**Cluster** runs multiple Node.js processes on the same server to use multiple CPU cores.

Each worker has its own:
- V8
- Event Loop
- Heap
- Memory

### Cluster vs Replicas

**Cluster** → multiple Node.js processes on the **same server**.

**Replicas** → multiple application instances, usually across servers/VMs/containers, often behind a load balancer.

### Easy Remember

    Worker Thread → CPU-heavy task
    Child Process → Separate program/process
    Cluster → Multiple Node.js processes
    Replica → Multiple application instances