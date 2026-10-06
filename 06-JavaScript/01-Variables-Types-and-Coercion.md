# JavaScript Variables, Types, and Coercion

## Variables and bindings

- `const` prevents reassignment of the binding; it does **not** freeze an object or array stored in that binding.
- `let` is for bindings that need reassignment.
- `var` is function-scoped and can be redeclared; prefer `const` or `let` in modern code.
- `let` and `const` are block-scoped and cannot be accessed before initialization because they are in the temporal dead zone (TDZ).

```javascript
const user = { name: "Ada" };
user.name = "Grace"; // allowed: object contents changed
// user = {};        // TypeError: const binding cannot be reassigned
```

## Data types

JavaScript has seven primitive types: `string`, `number`, `bigint`, `boolean`, `undefined`, `symbol`, and `null`. Objects include plain objects, arrays, dates, maps, sets, and functions.

```javascript
typeof "hello";       // "string"
typeof 42;             // "number"
typeof 42n;            // "bigint"
typeof true;           // "boolean"
typeof undefined;      // "undefined"
typeof Symbol("id");  // "symbol"
typeof null;           // "object" (historical JavaScript quirk)
Array.isArray([]);     // true
```

Primitives are immutable values. Objects are reference values; assigning an object variable copies its reference, not the object.

```javascript
const first = { score: 1 };
const second = first;
second.score = 2;
console.log(first.score); // 2
```

## Equality and coercion

- Prefer `===` and `!==` because they compare without implicit type conversion.
- `==` may convert operand types before comparing; know common examples, but avoid relying on surprising coercions.
- `Number()`, `String()`, and `Boolean()` make conversions explicit. Invalid numeric parsing can produce `NaN`; `Number.isNaN(value)` checks for it.
- `+` adds numbers but concatenates if either operand becomes a string.

```javascript
"5" == 5;      // true: coercion
"5" === 5;     // false: different types
Number("5");   // 5
"5" + 2;       // "52"
"5" - 2;       // 3 (numeric coercion)
```

## Truthy and falsy values

Falsy values include `false`, `0`, `-0`, `0n`, `""`, `null`, `undefined`, and `NaN`. All objects, including empty arrays and empty objects, are truthy.

```javascript
Boolean([]);   // true
Boolean({});   // true
Boolean("");   // false
```

Use `||` for a fallback on any falsy value; use `??` to fall back only for `null` or `undefined`.

```javascript
const count = 0;
count || 10; // 10
count ?? 10; // 0
```

## Operators and control flow

Know arithmetic, comparison, logical short-circuiting (`&&`, `||`, `??`), ternary expressions, optional chaining (`?.`), and nullish assignment (`??=`). Use `for...of` to iterate values; `for...in` enumerates object keys and is usually not the right choice for arrays.

```javascript
const displayName = user.profile?.name ?? "Anonymous";

for (const value of [2, 4, 6]) {
  console.log(value);
}
```

For interviews, be ready to predict coercion, compare `==` with `===`, explain primitive-vs-reference behavior, and distinguish `||` from `??`.
