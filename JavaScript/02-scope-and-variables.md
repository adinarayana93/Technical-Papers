# Scope and Variables in JavaScript

## 1. What is Scope?

**Scope** means the area of a program where a variable can be accessed.

Example:

```js
let name = "Adi";

function greet() {
    console.log(name);
}

greet();
```

`name` is accessible inside `greet()` because the function can access variables from its outer scope.

---

## 2. Types of Scope

### Global Scope

A variable declared outside functions/blocks is in global scope.

```js
let name = "Adi";

function greet() {
    console.log(name);
}
```

Global variables can be accessed from many places.

**Why avoid unnecessary global variables?**
- They can be changed accidentally.
- Different parts of the program can depend on them.
- They make code harder to understand and maintain.

---

### Function Scope

Variables declared with `var` inside a function are available throughout that function.

```js
function test() {
    var age = 25;
    console.log(age);
}

test();
// console.log(age); // ReferenceError
```

`age` exists only inside `test()`.

---

### Block Scope

A block is code inside `{ }`, such as an `if`, `for`, or `while`.

`let` and `const` are block-scoped.

```js
if (true) {
    let age = 25;
    const name = "Adi";

    console.log(age);
}

// console.log(age);  // ReferenceError
// console.log(name); // ReferenceError
```

---

# 3. `var`, `let`, and `const`

## `var`

```js
var age = 25;
age = 26;
```

`var` can be redeclared and reassigned.

```js
var x = 10;
var x = 20; // allowed
```

**Why generally avoid `var`?**

It is function-scoped, can be redeclared, and its hoisting behavior can cause confusing bugs.

---

## `let`

```js
let age = 25;
age = 26;
```

`let`:
- is block-scoped
- can be reassigned
- cannot be redeclared in the same scope

```js
let x = 10;
x = 20;       // allowed
// let x = 30; // SyntaxError
```

Use `let` when the value needs to change.

---

## `const`

```js
const age = 25;
// age = 26; // TypeError
```

`const`:
- is block-scoped
- cannot be reassigned
- cannot be redeclared in the same scope

Use `const` when the variable should not be reassigned.

### Important: `const` object/array contents can still change

```js
const user = { name: "Adi" };

user.name = "Rahul"; // allowed
```

The variable `user` cannot point to a different object, but the object's contents can change.

---

## Quick Comparison

| Feature | `var` | `let` | `const` |
|---|---|---|---|
| Function scoped | Yes | No | No |
| Block scoped | No | Yes | Yes |
| Reassign | Yes | Yes | No |
| Redeclare same scope | Yes | No | No |
| Recommended | Usually no | Yes | Yes |

---

# 4. Hoisting

**Hoisting** means JavaScript processes declarations before executing the code in that scope.

Example with `var`:

```js
console.log(age);
var age = 25;
```

Output:

```text
undefined
```

Conceptually, it behaves like:

```js
var age;
console.log(age);
age = 25;
```

The declaration is available before the assignment.

---

## Function Hoisting

Function declarations are hoisted.

```js
greet();

function greet() {
    console.log("Hello");
}
```

Output:

```text
Hello
```

---

# 5. Temporal Dead Zone (TDZ)

`let` and `const` are also hoisted, but they cannot be accessed before their declaration is reached.

The period between entering the scope and reaching the declaration is called the **Temporal Dead Zone (TDZ)**.

```js
console.log(age);
let age = 25;
```

Output:

```text
ReferenceError
```

Same with `const`:

```js
console.log(name);
const name = "Adi";
```

Output:

```text
ReferenceError
```

### Remember

```text
var   → hoisted, initialized as undefined
let   → hoisted, but in TDZ
const → hoisted, but in TDZ
```

---

# 6. Variable Naming

Use meaningful names.

Good:

```js
const studentName = "Adi";
const totalMarks = 450;
```

Bad:

```js
const x = "Adi";
const a = 450;
```

Common rules:
- Use camelCase: `studentName`
- Do not start with a number.
- Do not use reserved keywords.
- Choose names that explain the value.

---

# 7. Important Review Examples

### Example 1

```js
let x = 10;

if (true) {
    let x = 20;
    console.log(x);
}

console.log(x);
```

Output:

```text
20
10
```

The two `x` variables belong to different block scopes.

### Example 2

```js
var x = 10;

if (true) {
    var x = 20;
}

console.log(x);
```

Output:

```text
20
```

`var` is not block-scoped.

---

# 8. Notebook Revision Notes

```text
Scope = where a variable can be accessed.

Global scope     → accessible broadly
Function scope   → mainly applies to var inside functions
Block scope      → let and const inside { }

var   → function scoped, avoid in modern JS
let   → block scoped, can reassign
const → block scoped, cannot reassign

Hoisting → declarations are processed before execution.

var → can be accessed before declaration as undefined
let/const → TDZ → accessing before declaration gives ReferenceError

Use meaningful variable names.
Prefer const by default.
Use let when reassignment is required.
Avoid unnecessary global variables.
```

# 9. MountBlue Review Questions

1. What is scope?
2. What are global, function, and block scope?
3. What is the difference between `var`, `let`, and `const`?
4. Why should we avoid `var`?
5. What is hoisting?
6. What is the Temporal Dead Zone?
7. Why does `var` return `undefined` before assignment?
8. Why do `let` and `const` give `ReferenceError` before declaration?
9. Can a `const` object's properties be changed?
10. Why are global variables generally discouraged?
11. What is the difference between reassignment and redeclaration?
12. Why should variable names be meaningful?
