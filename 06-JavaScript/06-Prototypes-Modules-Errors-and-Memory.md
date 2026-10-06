# JavaScript Prototypes, Modules, Errors, and Memory

## Objects and prototypes

Objects can inherit properties from another object through their internal prototype link. When a property is not found on the object itself, JavaScript looks up the **prototype chain**.

```javascript
const base = { describe() { return "base"; } };
const item = Object.create(base);
item.name = "example";
item.describe(); // "base" (found on base)
Object.hasOwn(item, "name"); // true
Object.hasOwn(item, "describe"); // false
```

`class` syntax provides a familiar way to define constructors and methods, but JavaScript class inheritance is built on prototypes.

```javascript
class User {
  constructor(name) { this.name = name; }
  greet() { return `Hi, ${this.name}`; }
}
class Admin extends User {
  canManage() { return true; }
}
```

Methods defined on a class's prototype are shared by instances rather than created anew on every instance. `instanceof` checks a prototype relationship; it may not be reliable across separate JavaScript realms.

## ES Modules and CommonJS

- **ES Modules (ESM):** `import` / `export`; static module structure; supports live bindings and top-level `await` in supported runtimes.
- **CommonJS (CJS):** `require()` / `module.exports`; common in older Node.js code and still supported by Node.js.
- A project's `package.json`, file extensions, and runtime configuration affect how Node interprets module files. Avoid casually mixing module systems; interop can have edge cases.

```javascript
// ESM
export function add(a, b) { return a + b; }
import { add } from "./math.js";
```

```javascript
// CommonJS
module.exports = { add };
const { add } = require("./math.cjs");
```

## Error handling

Use `throw` to report failure and `try`/`catch` to handle it at a boundary where recovery or useful context is available. `finally` is for cleanup that must happen regardless of success or failure.

```javascript
try {
  const value = JSON.parse(input);
  processValue(value);
} catch (error) {
  logger.error({ error }, "Could not process input");
  throw error;
} finally {
  releaseResource();
}
```

Prefer `Error` objects over throwing strings, preserve causes when wrapping errors, and avoid swallowing failures without a deliberate fallback.

## Memory and garbage collection

JavaScript engines automatically reclaim objects that are no longer reachable from live references. Garbage collection does not prevent leaks: an object remains live while reachable through a global variable, closure, cache, event listener, timer, or other reference.

Common leak sources include unbounded caches, listeners that are never removed, timers that are never cleared, and closures retaining large object graphs. `WeakMap` and `WeakSet` do not keep object keys alive by themselves, but they are not a universal substitute for lifecycle cleanup.

## Interview reminders

Explain the prototype chain, how class syntax relates to prototypes, the module system used by the application, where errors should be handled, and how unwanted references can cause memory leaks.
