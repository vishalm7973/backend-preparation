# JavaScript Prototypes, Modules, Errors, and Memory

## 1. Objects & Prototypes

Objects can inherit properties and methods from another object.

If JavaScript cannot find a property on the object, it looks up the **prototype chain**.

    const base = {
      greet() {
        return "Hello";
      }
    };

    const user = Object.create(base);

    user.name = "John";

    console.log(user.name);
    // John

    console.log(user.greet());
    // Hello

`greet()` is not directly inside `user`. JavaScript finds it in `base`.

### Remember

    Object property found?
    ↓
    Yes → Use it
    No  → Search prototype chain

---

## 2. Classes & Prototypes

JavaScript `class` is mainly a cleaner syntax built on top of prototypes.

    class User {
      constructor(name) {
        this.name = name;
      }

      greet() {
        return `Hello ${this.name}`;
      }
    }

    const user = new User("John");

    console.log(user.greet());
    // Hello John

Methods like `greet()` are stored on the class prototype and shared by instances.

### Inheritance

    class Admin extends User {
      canManage() {
        return true;
      }
    }

    const admin = new Admin("Alex");

    admin.greet();
    // Hello Alex

    admin.canManage();
    // true

### `instanceof`

Checks whether an object is related to a constructor's prototype.

    admin instanceof Admin;
    // true

    admin instanceof User;
    // true


# 3. ES Modules vs CommonJS

JavaScript has two common module systems in Node.js.

### ES Modules (ESM)

Uses `import` and `export`.

    // math.js
    export function add(a, b) {
      return a + b;
    }

    // app.js
    import { add } from "./math.js";

    console.log(add(2, 3));
    // 5

### CommonJS (CJS)

Uses `require()` and `module.exports`.

    // math.js
    function add(a, b) {
      return a + b;
    }

    module.exports = { add };

    // app.js
    const { add } = require("./math");

    console.log(add(2, 3));
    // 5

### Remember

    ESM → import / export
    CJS → require / module.exports


# 4. Error Handling

Use `throw` to create/report an error.

Use `try...catch` to handle errors.

Use `finally` for cleanup.

    try {
      const data = JSON.parse(input);

      console.log(data);
    } catch (error) {
      console.log("Invalid JSON");
    } finally {
      console.log("Done");
    }

### Example

    function divide(a, b) {
      if (b === 0) {
        throw new Error("Cannot divide by zero");
      }

      return a / b;
    }

    try {
      divide(10, 0);
    } catch (error) {
      console.log(error.message);
    }

Prefer throwing `Error` objects instead of strings.

    throw new Error("Something went wrong");


# 5. Memory & Garbage Collection

JavaScript automatically removes objects that are **no longer reachable**.

This process is called **Garbage Collection (GC)**.

    let user = {
      name: "John"
    };

    user = null;

The object is no longer reachable through `user`, so it can eventually be cleaned by the garbage collector.

### Important

Garbage collection does **not** prevent memory leaks.

If something still holds a reference to an object, it can remain in memory.

### Common Memory Leak Sources

- Global variables
- Unbounded caches
- Event listeners not removed
- Timers not cleared
- Closures holding large objects


If `cache` keeps growing forever, memory usage can keep increasing.

### WeakMap / WeakSet

`WeakMap` and `WeakSet` do not keep their object keys alive by themselves.

    const weakMap = new WeakMap();

    let user = {
      name: "John"
    };

    weakMap.set(user, "data");

    user = null;

The object can now become eligible for garbage collection.