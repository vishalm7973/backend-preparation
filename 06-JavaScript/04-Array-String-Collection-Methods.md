# JavaScript Array, String, and Collection Methods

This is a practical interview catalog, not a list of every built-in method. Check whether a method mutates its receiver; mutation is a frequent interview detail.

## Array methods

Assume `const numbers = [1, 2, 3, 4]` and `const users = [{ id: 1 }, { id: 2 }]`.

### Iterate and transform

| Method | What it does | Syntax / example |
|---|---|---|
| `forEach` | Runs a callback for each present element; returns `undefined`. | `numbers.forEach((n, i) => console.log(i, n));` |
| `map` | Creates a new array from callback results; does not mutate the source. | `const doubled = numbers.map((n) => n * 2);` |
| `filter` | Creates a new array containing elements that pass a predicate. | `const even = numbers.filter((n) => n % 2 === 0);` |
| `reduce` | Combines values into one result; pass an initial value to handle empty arrays and make the accumulator type clear. | `const total = numbers.reduce((sum, n) => sum + n, 0);` |
| `reduceRight` | Reduces from right to left. | `const joined = ["a", "b", "c"].reduceRight((out, x) => out + x, "");` |
| `flat` | Flattens nested arrays to the requested depth (default `1`). | `[[1], [2, [3]]].flat(2); // [1, 2, 3]` |
| `flatMap` | Maps each item, then flattens the result one level. | `[1, 2].flatMap((n) => [n, n * 2]); // [1, 2, 2, 4]` |

`map` is for transforming each element; `forEach` is for side effects. `reduce` can express many transformations, but prefer `map` or `filter` when they state the intent more clearly.

### Search and test

| Method | What it does | Syntax / example |
|---|---|---|
| `find` | Returns the first matching element, or `undefined`. | `users.find((user) => user.id === 2);` |
| `findIndex` | Returns the first matching index, or `-1`. | `users.findIndex((user) => user.id === 2);` |
| `findLast` | Returns the last matching element (newer runtimes). | `numbers.findLast((n) => n % 2 === 0);` |
| `some` | Tests whether at least one element passes a predicate. | `numbers.some((n) => n > 3); // true` |
| `every` | Tests whether all elements pass a predicate. | `numbers.every((n) => n > 0); // true` |
| `includes` | Tests whether an array contains a value (uses SameValueZero equality). | `[1, NaN].includes(NaN); // true` |
| `indexOf` | Returns the first strict-equality match index, or `-1`. | `numbers.indexOf(3); // 2` |
| `lastIndexOf` | Returns the last strict-equality match index, or `-1`. | `[1, 2, 1].lastIndexOf(1); // 2` |

### Copy and combine

| Method | What it does | Syntax / example |
|---|---|---|
| `slice(start, end)` | Returns a shallow copy of a range; does not change the source; `end` is excluded. | `numbers.slice(1, 3); // [2, 3]` |
| `concat` | Returns a new array containing the source and added values. | `[1, 2].concat([3]); // [1, 2, 3]` |
| `join` | Joins array items into a string. | `["a", "b"].join("-"); // "a-b"` |
| `toSorted` | Returns a sorted copy (newer runtimes); comparator is recommended for numbers. | `numbers.toSorted((a, b) => a - b);` |
| `toReversed` | Returns a reversed copy (newer runtimes). | `numbers.toReversed();` |
| `toSpliced` | Returns a copy with elements added or removed (newer runtimes). | `numbers.toSpliced(1, 1, 9);` |
| `with(index, value)` | Returns a copy with one index replaced (newer runtimes). | `numbers.with(1, 9); // [1, 9, 3, 4]` |

### Mutating methods

| Method | What it does | Syntax / example |
|---|---|---|
| `push` | Adds to the end; returns the new length. | `numbers.push(5);` |
| `pop` | Removes and returns the last element. | `numbers.pop();` |
| `unshift` | Adds to the beginning; returns the new length. | `numbers.unshift(0);` |
| `shift` | Removes and returns the first element. | `numbers.shift();` |
| `splice(start, deleteCount, ...items)` | Removes/replaces/inserts in place; returns removed elements. | `numbers.splice(1, 2, 9);` |
| `sort(compareFn)` | Sorts in place and returns the same array. Default sorting is lexicographic, so use a comparator for numbers. | `numbers.sort((a, b) => a - b);` |
| `reverse` | Reverses in place and returns the same array. | `numbers.reverse();` |
| `fill(value, start, end)` | Replaces a range in place with a value. | `Array(3).fill(0); // [0, 0, 0]` |
| `copyWithin(target, start, end)` | Copies part of the array onto another range in place. | `[1, 2, 3, 4].copyWithin(1, 2); // [1, 3, 4, 4]` |

### `slice` vs. `splice`

- `slice(start, end)` returns a shallow copy and leaves the source unchanged.
- `splice(start, deleteCount, ...items)` changes the source by removing, replacing, or inserting elements.

```javascript
const values = [10, 20, 30, 40];
values.slice(1, 3);       // [20, 30], values unchanged
values.splice(1, 2, 99);  // returns [20, 30]; values becomes [10, 99, 40]
```

### Construction and checks

| Method | What it does | Syntax / example |
|---|---|---|
| `Array.isArray(value)` | Reliably checks whether a value is an array. | `Array.isArray([1, 2]); // true` |
| `Array.from(iterable, mapFn?)` | Creates an array from an iterable or array-like object; can map while creating. | `Array.from("cat"); // ["c", "a", "t"]` |
| `Array.of(...items)` | Creates an array from its arguments. | `Array.of(3); // [3]` |
| `at(index)` | Reads by positive or negative index. | `numbers.at(-1); // 4` |

## String methods

Strings are immutable: these methods return values rather than changing the original string.

| Method | What it does | Syntax / example |
|---|---|---|
| `includes(text)` | Checks whether a substring exists. | `"backend".includes("end"); // true` |
| `startsWith(text)` / `endsWith(text)` | Checks a prefix or suffix. | `"api.json".endsWith(".json"); // true` |
| `slice(start, end)` | Returns a substring; supports negative offsets; end is excluded. | `"backend".slice(0, 3); // "bac"` |
| `substring(start, end)` | Returns a range; negative arguments are treated as zero. | `"backend".substring(0, 3); // "bac"` |
| `split(separator)` | Splits into an array. | `"a,b,c".split(",");` |
| `replace(search, replacement)` | Replaces the first string match or regex match. | `"a-a".replace("a", "b"); // "b-a"` |
| `replaceAll(search, replacement)` | Replaces all string matches. | `"a-a".replaceAll("a", "b"); // "b-b"` |
| `trim()` | Removes whitespace from both ends. | `"  hi  ".trim(); // "hi"` |
| `toLowerCase()` / `toUpperCase()` | Changes letter case in the returned string. | `"Hi".toLowerCase(); // "hi"` |
| `padStart(length, text)` / `padEnd(length, text)` | Pads to a target length. | `"7".padStart(3, "0"); // "007"` |

## Object and collection methods

| API | What it does | Syntax / example |
|---|---|---|
| `Object.keys(obj)` | Returns an array of an object's own enumerable string keys. | `Object.keys({ a: 1 }); // ["a"]` |
| `Object.values(obj)` | Returns own enumerable values. | `Object.values({ a: 1 }); // [1]` |
| `Object.entries(obj)` | Returns `[key, value]` pairs. | `Object.entries({ a: 1 }); // [["a", 1]]` |
| `Object.fromEntries(pairs)` | Builds an object from key/value pairs. | `Object.fromEntries([["a", 1]]);` |
| `Object.hasOwn(obj, key)` | Checks for an own property, not one inherited through the prototype. | `Object.hasOwn({ a: 1 }, "a"); // true` |
| `Map.set(key, value)` / `get(key)` / `has(key)` / `delete(key)` | Stores key/value pairs with keys of any type. | `const map = new Map(); map.set("id", 1); map.get("id");` |
| `Set.add(value)` / `has(value)` / `delete(value)` | Stores unique values. | `const unique = new Set([1, 1, 2]); unique.size; // 2` |

Interview checklist: know the return value, mutation behavior, callback arguments, and whether the operation is shallow or deep. Exact browser/runtime support can vary for newer methods such as `toSorted`, `findLast`, and `with`.
