# JavaScript Variables, Types, and Coercion

## 1. Variables

- `const` → Cannot reassign the variable.
- `let` → Can be reassigned.
- `var` → Function-scoped; avoid in modern code.
- `let` and `const` → Block-scoped.
- `let` and `const` → Cannot be used before initialization (TDZ).

### Example

    const name = "John";
    name = "Alex"; // Error

    let age = 25;
    age = 26; // Allowed

    var city = "Delhi";

### Block Scope

    {
      let a = 10;
      const b = 20;
    }

    console.log(a); // Error
    console.log(b); // Error

`let` and `const` are only available inside the block.

### TDZ (Temporal Dead Zone)

    console.log(age); // Error
    let age = 25;

The variable exists but cannot be accessed before its declaration.

### Important

`const` does NOT make objects/arrays immutable.

    const user = {
      name: "Ada"
    };

    user.name = "Grace"; // Allowed

You cannot reassign the variable:

    user = {}; // Error


## 2. Data Types

### Primitive Types

- `string`
- `number`
- `bigint`
- `boolean`
- `undefined`
- `symbol`
- `null`

### Non-Primitive / Objects

- Object
- Array
- Function
- Date
- Map
- Set

### Remember

    Primitive → Value
    Object → Reference

### Example

    const first = { score: 1 };
    const second = first;

    second.score = 2;

    console.log(first.score); // 2

Both variables point to the same object.


## 3. Shallow Copy vs Deep Copy

### Shallow Copy

A shallow copy copies only the first level.

Nested objects still share the same reference.

    const user = {
      name: "John",
      address: {
        city: "Delhi"
      }
    };

    const copy = { ...user };

    copy.name = "Alex";
    copy.address.city = "Mumbai";

    console.log(user.name); // "John"
    console.log(user.address.city); // "Mumbai"

The nested `address` is still shared.

### Common Ways to Create a Shallow Copy

    const copy1 = { ...user };

    const copy2 = Object.assign({}, user);

### Deep Copy

A deep copy creates completely independent copies, including nested objects.

    const user = {
      name: "John",
      address: {
        city: "Delhi"
      }
    };

    const copy = structuredClone(user);

    copy.address.city = "Mumbai";

    console.log(user.address.city); // "Delhi"
    console.log(copy.address.city); // "Mumbai"

### Remember

    Shallow copy → First level copied, nested references shared
    Deep copy    → Nested objects are also copied

### Interview

> A shallow copy does not copy nested objects independently, while a deep copy creates independent copies of nested objects.


## 4. Equality & Type Coercion

Prefer:

    ===
    !==

`==` can automatically convert types.

### Example

    "5" == 5;   // true
    "5" === 5;  // false

### Explicit Conversion

    Number("5");   // 5
    String(5);      // "5"
    Boolean(1);     // true

### `+` vs `-`

    "5" + 2; // "52"
    "5" - 2; // 3

Remember:

    + → Can concatenate strings
    - → Converts to number


## 5. Truthy & Falsy

Common falsy values:

    false
    0
    ""
    null
    undefined
    NaN

### Examples

    Boolean([]);  // true
    Boolean({});  // true
    Boolean("");  // false
    Boolean(0);   // false

Objects and arrays are truthy even when they are empty.


## 6. `||` vs `??`

### `||`

Fallback for any **falsy** value.

    const count = 0;

    console.log(count || 10);
    // 10

### `??`

Fallback only for `null` or `undefined`.

    const count = 0;

    console.log(count ?? 10);
    // 0

### Remember

    || → falsy
    ?? → null / undefined


## 7. Useful Operators

- `&&` → AND
- `||` → OR / fallback
- `??` → null/undefined fallback
- `?.` → Optional chaining
- `??=` → Nullish assignment
- `?:` → Ternary

### Example

    const user = {
      profile: {
        name: "John"
      }
    };

    const name = user.profile?.name ?? "Anonymous";

    console.log(name);
    // John

### Optional Chaining

Prevents errors when a property does not exist.

    const user = {};

    console.log(user.profile?.name);
    // undefined


## 8. Loops

Use `for...of` to get **values**.

    for (const value of [2, 4, 6]) {
      console.log(value);
    }

    // 2
    // 4
    // 6

Use `for...in` to get **keys/indexes**.

    const user = {
      name: "John",
      age: 25
    };

    for (const key in user) {
      console.log(key);
    }

    // name
    // age

For arrays, `for...in` gives the **indexes**.

    for (const index in [2, 4, 6]) {
      console.log(index);
    }

    // 0
    // 1
    // 2

### Remember

    for...of → Values
    for...in → Keys / Indexes