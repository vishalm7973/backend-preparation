# JavaScript Array, String, and Collection Methods

Practical interview methods to remember.

---

## 1. Array Methods

Assume:

    const numbers = [1, 2, 3, 4];

### Iterate & Transform

### `forEach()`
Runs a function for each element. Returns `undefined`.

    numbers.forEach((n) => {
      console.log(n);
    });

    // 1
    // 2
    // 3
    // 4

Use when you want to **do something**, not create a new array.

---

### `map()`
Creates a **new array** by transforming each element.

    const doubled = numbers.map((n) => n * 2);

    console.log(doubled);
    // [2, 4, 6, 8]

---

### `filter()`
Creates a new array containing elements that match a condition.

    const even = numbers.filter((n) => n % 2 === 0);

    // [2, 4]

---

### `reduce()`
Combines all values into **one result**.

    const total = numbers.reduce((acc, value) => acc + value, 0);

    // 10
---

### `flat()`
Flattens nested arrays and return new array.

    const values = [1, [2, [3]]];

    values.flat(2);

    // [1, 2, 3]

---

### `flatMap()`
Maps and then flattens one level and return new array.

    [1, 2].flatMap((n) => [n, n * 2]);

    // [1, 2, 2, 4]

---

## 2. Search & Test

### `find()`
Returns the **first matching element**.

    const users = [
      { id: 1, name: "John" },
      { id: 2, name: "Alex" }
    ];

    users.find((user) => user.id === 2);

    // { id: 2, name: "Alex" }

If nothing is found → `undefined`.

---

### `findIndex()`
Returns the index of the first match.

    numbers.findIndex((n) => n === 3);

    // 2

If not found → `-1`.

---

### `some()`
Checks if **at least one** element matches.

    numbers.some((n) => n > 3);

    // true

---

### `every()`
Checks if **all** elements match.

    numbers.every((n) => n > 0);

    // true

---

### `includes()`
Checks whether an array contains a value.

    [1, 2, 3].includes(2);

    // true

---

### `indexOf()`
Returns the first matching index.

    numbers.indexOf(3);

    // 2

If not found → `-1`.

---

### `lastIndexOf()`
Returns the last matching index.

    [1, 2, 1].lastIndexOf(1);

    // 2

---

## 3. Copy & Combine

### `slice()`

Returns part of an array.

**Does not mutate the original array.**

    const numbers = [1, 2, 3, 4];

    numbers.slice(1, 3);

    // [2, 3]

`end` is not included.

---

### `concat()`

Combines arrays and returns a new array.

    [1, 2].concat([3, 4]);

    // [1, 2, 3, 4]

---

### `join()`

Converts array elements into a string.

    ["a", "b", "c"].join("-");

    // "a-b-c"

---

## 4. Mutating Methods

These methods **change the original array**.

### `push()`
Adds to the end.

    const numbers = [1, 2];

    numbers.push(3);

    // [1, 2, 3]

---

### `pop()`
Removes the last element.

    const numbers = [1, 2, 3];

    numbers.pop();

    // [1, 2]

---

### `unshift()`
Adds to the beginning.

    const numbers = [2, 3];

    numbers.unshift(1);

    // [1, 2, 3]

---

### `shift()`
Removes the first element.

    const numbers = [1, 2, 3];

    numbers.shift();

    // [2, 3]

---

### `splice()`

Adds, removes, or replaces elements **in the original array**.

    const numbers = [10, 20, 30, 40];

    numbers.splice(1, 2, 99);

    console.log(numbers);

    // [10, 99, 40]

---

### `sort()`

Sorts the original array.

For numbers, use a comparator.

    const numbers = [10, 2, 5];

    numbers.sort((a, b) => a - b);

    // [2, 5, 10]

Without a comparator, numbers are sorted like strings.

---

### `reverse()`

Reverses the original array.

    const numbers = [1, 2, 3];

    numbers.reverse();

    // [3, 2, 1]

---

### `fill()`

Replaces values in a range.

    const numbers = [1, 2, 3];

    numbers.fill(0);

    // [0, 0, 0]

---

## 5. `slice()` vs `splice()`

### `slice()`

- Does NOT change original array.
- Returns a copy/range.

    const numbers = [10, 20, 30, 40];

    const result = numbers.slice(1, 3);

    // result → [20, 30]
    // numbers → [10, 20, 30, 40]

### `splice()`

- Changes original array.
- Can remove, replace, or insert.

    const numbers = [10, 20, 30, 40];

    numbers.splice(1, 2, 99);

    // numbers → [10, 99, 40]

### Remember

    slice  → Copy → No mutation
    splice → Modify → Mutation

---

## 6. Useful Array Methods

### `Array.isArray()`

Checks whether a value is an array.

    Array.isArray([1, 2]);

    // true

---

### `Array.from()`

Creates an array from an iterable or array-like value.

    Array.from("cat");

    // ["c", "a", "t"]

---

### `Array.of()`

Creates an array from arguments.

    Array.of(3);

    // [3]

---

### `at()`

Gets an element using an index.

    const numbers = [1, 2, 3, 4];

    numbers.at(-1);

    // 4

Negative indexes start from the end.

---

# 7. String Methods

Strings are **immutable**.

Methods return a new value instead of changing the original string.

### `includes()`

Checks whether text exists.

    "backend".includes("end");

    // true

---

### `startsWith()` / `endsWith()`

Checks the beginning or end.

    "api.json".endsWith(".json");

    // true

---

### `slice()`

Returns part of a string.

    "backend".slice(0, 3);

    // "bac"

---

### `substring()`

Returns part of a string.

    "backend".substring(0, 3);

    // "bac"

---

### `split()`

Converts a string into an array.

    "a,b,c".split(",");

    // ["a", "b", "c"]

---

### `replace()`

Replaces the first matching string.

    "a-a".replace("a", "b");

    // "b-a"

---

### `replaceAll()`

Replaces all matching strings.

    "a-a".replaceAll("a", "b");

    // "b-b"

---

### `trim()`

Removes whitespace from both ends.

    "  hello  ".trim();

    // "hello"

---

### `toLowerCase()` / `toUpperCase()`

Changes letter case.

    "Hello".toLowerCase();

    // "hello"

---

### `padStart()`

Adds characters to the beginning.

    "7".padStart(3, "0");

    // "007"


# 8. Object Methods

### `Object.keys()`

Returns object keys.

    const user = {
      name: "John",
      age: 25
    };

    Object.keys(user);

    // ["name", "age"]

---

### `Object.values()`

Returns object values.

    Object.values(user);

    // ["John", 25]

---

### `Object.entries()`

Returns key-value pairs.

    Object.entries(user);

    // [["name", "John"], ["age", 25]]

---

### `Object.fromEntries()`

Creates an object from key-value pairs.

    Object.fromEntries([
      ["name", "John"],
      ["age", 25]
    ]);

    // { name: "John", age: 25 }

---

### `Object.hasOwn()`

Checks whether an object directly owns a property.

    const user = {
      name: "John"
    };

    Object.hasOwn(user, "name");

    // true


# 9. Map

`Map` stores **key-value pairs**.

Keys can be of any type.

    const map = new Map();

    map.set("name", "John");

    map.get("name");

    // John

Useful methods:

    map.set(key, value);
    map.get(key);
    map.has(key);
    map.delete(key);


# 10. Set

`Set` stores **unique values**.

    const numbers = new Set([1, 1, 2, 3]);

    console.log(numbers);

    // Set {1, 2, 3}

    numbers.has(2);

    // true

Useful for removing duplicates:

    const numbers = [1, 1, 2, 2, 3];

    const unique = [...new Set(numbers)];

    // [1, 2, 3]