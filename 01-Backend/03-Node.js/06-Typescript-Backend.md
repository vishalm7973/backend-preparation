# TypeScript

## 1. What is TypeScript?

TypeScript is a **superset of JavaScript** that adds **static typing** and type checking.

It helps catch errors before the application runs.

    TypeScript
        ↓
    Type checking
        ↓
    JavaScript
        ↓
    Node.js / Browser
        ↓
    Runtime

**Remember:** TypeScript checks types; JavaScript runs at runtime.

---

## 2. Static vs Dynamic Typing

### Static Typing
Types are checked **before runtime**.

Example: TypeScript

### Dynamic Typing
Types are checked **at runtime**.

Example: JavaScript

**Remember:**

    TypeScript → Type checking before runtime
    JavaScript  → Type checking during runtime

---

## 3. Compile Time vs Runtime

**Compile time** → Before the program runs.

Example: TypeScript checks types.

**Runtime** → When the program is actually running.

Example:
- Functions execute
- API requests happen
- DB queries run
- Files are read

---

## 4. TypeScript Strict Options

### `strictNullChecks`
Makes TypeScript handle `null` and `undefined` separately.

### `noImplicitAny`
Prevents TypeScript from automatically using `any`.

### `strictFunctionTypes`
Checks function types more strictly.

### `strictPropertyInitialization`
Ensures class properties are initialized.

### `noImplicitThis`
Checks that `this` is used correctly.

### `strict: true`
Enables the main strict type-checking rules together.

**Remember:** `strict: true` = stronger type checking.

---

## 5. Generics

Generics allow us to write reusable code while keeping type safety.

    function getValue<T>(value: T): T {
      return value;
    }

`T` is just a name for the generic type.

**Simple:**  
"I don't know the type yet. I'll know it when the function is used."

---

## 6. Utility Types

Utility types modify existing types.

| Utility | Simple meaning |
|---|---|
| `Partial<T>` | Everything optional |
| `Required<T>` | Everything required |
| `Readonly<T>` | Cannot reassign properties |
| `Pick<T,K>` | Keep selected properties |
| `Omit<T,K>` | Remove selected properties |
| `Record<K,T>` | Key-value object type |
| `ReturnType<T>` | Get function return type |
| `Parameters<T>` | Get function parameters |

### Example

    interface User {
      id: number;
      name: string;
      email: string;
    }

    type UpdateUser = Partial<User>;

Now all fields are optional.

**Common use:** `Partial<T>` → PATCH/update APIs.

---

## 7. Type Narrowing

Narrowing means making a broad type more specific using a check.

    function print(value: string | number) {
      if (typeof value === "string") {
        console.log(value.toUpperCase());
      } else {
        console.log(value.toFixed(2));
      }
    }

TypeScript knows the correct type inside each block.

---

## 8. DTO

**DTO = Data Transfer Object**

It defines the structure of data sent between the client and backend.

    interface CreateUserDto {
      name: string;
      email: string;
      password: string;
    }

### Why DTO?

- Clear API contract
- Better type safety
- Defines expected fields
- Keeps API data separate from DB entities

**Remember:** DTO = "What data does this API expect?"

---

## 9. Compile-time Type Checking

TypeScript checks code **before the application runs**.

    TypeScript code
         ↓
    Type checking
         ↓
    Error found / JavaScript generated

---

## 10. Runtime Validation

Runtime validation checks **actual data while the application is running**.

Example:

    Client → API
             ↓
         Validate body
             ↓
         Service

It checks things like:
- Required fields
- Correct types
- Valid format

### Important Difference

**TypeScript type checking** → before runtime.

**Runtime validation** → when real data enters the application.