# JavaScript Basics


# 1. Data Types in JavaScript

A **data type** tells us what kind of value we are working with.

## String

Used for text.

```js
const name = "Bruce";
const city = "Gotham";

console.log(typeof name); // "string"
```

## Number

Used for integers and decimal numbers.

```js
const age = 36;
const price = 99.50;

console.log(typeof age);   // "number"
console.log(typeof price); // "number"
```

JavaScript normally uses one `Number` type for both integers and floating-point numbers.

## Boolean

A Boolean has only two values:

```js
true
false
```

Example:

```js
const isLoggedIn = true;

if (isLoggedIn) {
    console.log("Welcome");
}
```

## Undefined

`undefined` usually means a value has not been assigned or is not available.

```js
let age;

console.log(age); // undefined
```

## Null

`null` represents an intentional absence of a value.

```js
const selectedUser = null;
```

A simple way to remember:

```text
undefined → value is not currently available/assigned
null      → intentionally saying "no value"
```

One JavaScript oddity:

```js
console.log(typeof null);
```

prints:

```text
object
```

This is a long-standing JavaScript behavior.

## Object

Objects store data as key-value pairs.

```js
const user = {
    name: "Bruce",
    age: 36
};

console.log(user.name); // Bruce
```

## Array

Arrays store ordered collections.

```js
const numbers = [10, 20, 30];

console.log(numbers[0]); // 10
```

Array indexes start at `0`.

Arrays are technically objects:

```js
console.log(typeof []); // "object"
```

To check specifically for an array:

```js
console.log(Array.isArray(numbers)); // true
```

## Function

Functions are reusable blocks of code.

```js
function greet() {
    console.log("Hello");
}

console.log(typeof greet); // "function"
```

## BigInt

`BigInt` is used for very large integers.

```js
const bigNumber = 123456789012345678901234567890n;

console.log(typeof bigNumber); // "bigint"
```

The `n` at the end makes it a BigInt.

## Symbol

A `Symbol` creates a unique value.

```js
const id = Symbol("id");

console.log(typeof id); // "symbol"
```

Symbols are mainly useful when unique property keys are needed.

---

# 2. Primitive and Reference Values

Common primitive types are:

```text
string
number
boolean
undefined
null
bigint
symbol
```

Objects, arrays, and functions are reference-type values.

Example:

```js
const age = 36;

const user = {
    name: "Bruce"
};

const numbers = [1, 2, 3];
```

This distinction becomes especially important when working with objects and arrays and when discussing how values are passed to functions.

---

# 3. `typeof`

`typeof` tells us the type of a value.

```js
console.log(typeof "hello");       // string
console.log(typeof 25);            // number
console.log(typeof true);          // boolean
console.log(typeof undefined);     // undefined
console.log(typeof null);          // object
console.log(typeof {});            // object
console.log(typeof []);            // object
console.log(typeof function () {}); // function
```

Important special cases:

```js
typeof null; // "object"
typeof [];   // "object"
```

For arrays, use:

```js
Array.isArray(value);
```

---

# 4. `null` vs `undefined`

This is a common review/interview question.

## `undefined`

Usually means the value has not been assigned or is not available.

```js
let username;

console.log(username); // undefined
```

## `null`

Usually means we intentionally represent "no value".

```js
let currentUser = null;
```

For example:

```js
let result;

if (result === undefined) {
    console.log("Result is undefined");
}
```

And:

```js
let currentUser = null;

if (currentUser === null) {
    console.log("No user is selected");
}
```

### Easy memory trick

```text
undefined → JavaScript has no value available here
null      → I intentionally set this to no value
```

---

# 5. Truthy and Falsy Values

When JavaScript uses a value in a condition, it converts that value to a Boolean.

Example:

```js
if (value) {
    console.log("Condition is true");
}
```

JavaScript asks:

> Is `value` truthy or falsy?

## Falsy values

These values are falsy:

```text
false
0
-0
0n
""
null
undefined
NaN
```

Example:

```js
if (0) {
    console.log("Runs");
}
```

It does not run because `0` is falsy.

## Truthy values

Almost everything else is truthy.

Examples:

```js
1
-1
"hello"
"0"
[]
{}
function () {}
```

For example:

```js
if ("hello") {
    console.log("Runs");
}
```

Output:

```text
Runs
```

Important:

```js
if ("0") {
    console.log("Runs");
}
```

This also runs because `"0"` is a non-empty string.

---

# 6. Why `value === undefined` Is Better Than `!value`

Suppose:

```js
const age = 0;
```

`0` is a valid number, but it is falsy.

If we write:

```js
if (!age) {
    console.log("Age is missing");
}
```

the message runs even though the age is actually `0`.

If we specifically want to check for `undefined`, use:

```js
if (age === undefined) {
    console.log("Age is undefined");
}
```

Now `0` does not trigger the condition.

The difference is:

```text
!value
→ checks whether value is falsy

value === undefined
→ specifically checks whether value is undefined
```

So don't use `!value` when the real requirement is specifically "is this `undefined`?"

---

# 7. `==` vs `===`

## `===` — strict equality

`===` compares both the value and the type.

```js
console.log(5 === 5);   // true
console.log(5 === "5"); // false
```

Why is the second one false?

```text
5   → number
"5" → string
```

The types are different.

## `==` — loose equality

`==` can perform type conversion before comparing.

```js
console.log(5 == "5"); // true
```

JavaScript converts values as part of the comparison.

Another example:

```js
console.log(false == 0); // true
```

This can create surprising results.

### Which should I normally use?

Prefer:

```js
===
```

because it is more predictable.

Remember:

```text
==  → loose equality, type conversion may happen
=== → strict equality, value and type must match
```

---

# 8. What Happens When a Function Has No `return`?

Consider:

```js
function greet() {
    console.log("Hello");
}

const result = greet();

console.log(result);
```

Output:

```text
Hello
undefined
```

The function runs, but it does not return a value.

Therefore:

> A function that does not return a value returns `undefined`.

Another example:

```js
function add(a, b) {
    const result = a + b;
}

const answer = add(10, 20);

console.log(answer);
```

Output:

```text
undefined
```

Even though `result` contains `30`, we never returned it.

Correct:

```js
function add(a, b) {
    const result = a + b;

    return result;
}

const answer = add(10, 20);

console.log(answer); // 30
```

---

# 9. `console.log()` vs `return`

These are different.

## `console.log()`

Displays something in the console.

```js
function add(a, b) {
    console.log(a + b);
}

add(10, 20);
```

Output:

```text
30
```

But:

```js
const result = add(10, 20);

console.log(result);
```

prints:

```text
30
undefined
```

because `add()` did not return the result.

## `return`

Sends a value back to the code that called the function.

```js
function add(a, b) {
    return a + b;
}

const result = add(10, 20);

console.log(result); // 30
```

Easy way to remember:

```text
console.log()
→ show something

return
→ give something back
```

---

# Final 2-Minute Revision

```text
JavaScript values have different data types.

Primitive:
string, number, boolean, undefined, null, bigint, symbol

Reference values:
objects, arrays, functions

typeof:
tells me the type of a value.

Special cases:
typeof null → "object"
typeof []   → "object"

Array check:
Array.isArray(value)

undefined:
value is not currently available/assigned.

null:
intentionally represents no value.

Falsy:
false, 0, -0, 0n, "", null, undefined, NaN

Truthy:
almost everything else.

==:
loose equality; type conversion may happen.

===:
strict equality; value and type must match.

Prefer ===.

For a specific undefined check:
value === undefined

Don't use !value for that because
0, false, "", null and NaN are also falsy.

Function without return:
returns undefined.

console.log:
shows something.

return:
gives a value back to the caller.
```
