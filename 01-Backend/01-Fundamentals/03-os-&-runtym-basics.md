# OS & Runtime Basics

## 1. Process

A **process** is a running instance of a program.

- Has its own memory and resources.
- Can be started, stopped, and restarted.

Example:

    Running Chrome = Process
    Running Node.js app = Process


## 2. Thread

A **thread** is a unit of execution inside a process.

- A process can have multiple threads.
- Threads share the process's memory.
- Threads are lighter than processes.

    Process
    ├── Thread 1
    ├── Thread 2
    └── Thread 3


## 3. Operating System (OS)

The **OS manages hardware and provides resources to programs**.

Examples:

- CPU
- Memory
- Files
- Network
- Processes
- Threads

Examples of OS: Windows, Linux, macOS.


## 4. Concurrency

**Concurrency = multiple tasks making progress during the same period.**

Tasks don't have to run at exactly the same time.

Example:

    Task A → Waiting for DB
    Task B → Processing
    Task A → Resumes

Node.js handles many I/O operations concurrently using the **event loop**.


## 5. Parallelism

**Parallelism = multiple tasks actually running at the same time.**

Usually achieved using multiple CPU cores.

### Concurrency vs Parallelism

    Concurrency → Multiple tasks are in progress.

    Parallelism → Multiple tasks execute at the same time.


## 6. CPU-Bound vs I/O-Bound

### CPU-Bound

Task spends most of its time using the **CPU**.

Examples:

- Image processing
- Video encoding
- Large calculations
- Encryption/compression

### I/O-Bound

Task spends most of its time **waiting for external resources**.

Examples:

- Database queries
- File operations
- Network requests
- External API calls

**Remember:**

    CPU-Bound → Waiting for CPU
    I/O-Bound  → Waiting for I/O


## 7. Memory

Memory is used by processes and threads while running.

A process mainly has:

    Process Memory
    ├── Code
    ├── Data
    ├── Stack
    └── Heap

### Stack

Used for:

- Function calls
- Local variables
- Return information

Stack follows **LIFO (Last In, First Out)**.

### Heap

Used for:

- Objects
- Arrays
- Dynamically allocated data

**Remember:**

    Stack → Function calls + Local data
    Heap  → Dynamic data


## 8. File Descriptor

A **file descriptor (FD)** is a number used by the OS to identify an open resource.

It can represent:

- File
- Socket
- Pipe
- Other I/O resources

Example:

    File opened → OS gives FD
    Program uses FD → Read / Write
    Program closes → FD released


## 9. OS Socket

A **socket is an OS-managed endpoint for network communication**.

It allows a program to:

- Listen for connections
- Accept connections
- Send data
- Receive data

Example:

    Client
       ↓
    Network
       ↓
    Server Socket
       ↓
    Server Application
