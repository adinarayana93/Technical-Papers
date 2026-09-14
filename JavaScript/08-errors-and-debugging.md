# Errors and Debugging in JavaScript

## 1. What is Debugging?

**Debugging** means finding and fixing problems (bugs) in a program.

A simple debugging process:

```text
Error happens
     ↓
Read the error message
     ↓
Find file + line
     ↓
Understand the cause
     ↓
Fix the root cause
     ↓
Run and test again
```

Do not just change random lines until the error disappears. Try to understand why it happened.

---

# 2. Common JavaScript Errors

## `SyntaxError`

The JavaScript code does not follow valid syntax.

```js
const name =
```

Output:

```text
SyntaxError
```

Common causes:
- Missing brackets
- Missing quotes
- Invalid syntax
- Incorrect punctuation

---

## `ReferenceError`

JavaScript cannot find the variable or identifier you are trying to use.

```js
console.log(username);
```

If `username` was never declared:

```text
ReferenceError: username is not defined
```

---

## `TypeError`

The value has the wrong type for the operation you are trying to perform.

```js
const name = "Adi";

name();
```

A string is not a function, so this causes a `TypeError`.

Another example:

```js
const user = null;

console.log(user.name);
```

Output:

```text
TypeError
```

---

# 3. How to Read a Stack Trace

Example:

```text
TypeError: user.getName is not a function
    at printUser (/project/app.js:10:15)
    at main (/project/app.js:20:5)
```

Look at:

```text
TypeError
    ↓
What kind of error?

user.getName is not a function
    ↓
What went wrong?

app.js:10:15
    ↓
File, line, column

printUser → main
    ↓
Function call path
```

A stack trace shows the path through function calls that led to the error.

### When debugging, check:

1. Error type
2. Error message
3. File name
4. Line number
5. Column number
6. Function/call stack
7. The values involved

---

# 4. `throw`

`throw` is used when we deliberately want to raise an error.

```js
function withdraw(balance, amount) {
    if (amount > balance) {
        throw new Error("Insufficient balance");
    }

    return balance - amount;
}

console.log(withdraw(1000, 1200));
```

The function stops at `throw` and an error is raised.

---

## Why `throw new Error()`?

Prefer:

```js
throw new Error("Invalid age");
```

instead of:

```js
throw "Invalid age";
```

`Error` objects provide useful information such as:

- `message`
- `name`
- `stack`

Example:

```js
const error = new Error("Something went wrong");

console.log(error.message);
console.log(error.name);
console.log(error.stack);
```

Output includes:

```text
Something went wrong
Error
...
```

Throwing strings or plain objects is possible, but `Error` objects are easier to debug and handle consistently.

---

# 5. `try...catch`

`try...catch` lets us handle an error instead of allowing it to terminate the normal flow of the program.

```js
try {
    const result = 10 / 0;
    console.log(result);
} catch (error) {
    console.log("An error occurred");
}
```

A more useful example:

```js
try {
    JSON.parse("invalid json");
} catch (error) {
    console.log("Could not parse JSON");
}
```

Output:

```text
Could not parse JSON
```

The error is caught by `catch`.

---

# 6. What Happens After an Error?

Look at this:

```js
try {
    console.log("Before");

    throw new Error("Problem");

    console.log("After");
} catch (error) {
    console.log("Caught");
}

console.log("Program continues");
```

Output:

```text
Before
Caught
Program continues
```

Important:

```text
throw
  ↓
remaining code inside try is skipped
  ↓
catch runs
  ↓
code after try/catch continues
```

So `catch` does **not** continue from the line immediately after `throw` inside the `try` block.

---

# 7. `finally`

`finally` runs whether an error occurs or not.

```js
try {
    console.log("Try");
} catch (error) {
    console.log("Catch");
} finally {
    console.log("Finally");
}
```

Output:

```text
Try
Finally
```

With an error:

```js
try {
    throw new Error("Problem");
} catch (error) {
    console.log("Catch");
} finally {
    console.log("Finally");
}
```

Output:

```text
Catch
Finally
```

`finally` is useful for cleanup work.

---

# 8. Error Properties

Inside `catch`:

```js
try {
    throw new Error("Invalid input");
} catch (error) {
    console.log(error.name);
    console.log(error.message);
    console.log(error.stack);
}
```

### Main properties

```text
error.name     → type of error
error.message  → explanation
error.stack    → error + call stack
```

---

# 9. Console Debugging Methods

## `console.log()`

General debugging information.

```js
console.log("User:", user);
```

## `console.error()`

Displays an error message.

```js
console.error("Failed to save user");
```

## `console.warn()`

Displays a warning.

```js
console.warn("Password is weak");
```

## `console.info()`

Displays informational output.

```js
console.info("Server started");
```

## `console.table()`

Very useful for arrays of objects.

```js
const users = [
    { name: "Adi", age: 25 },
    { name: "Rahul", age: 30 }
];

console.table(users);
```

It displays the data in table form.

## `console.dir()`

Useful for inspecting objects.

```js
console.dir(user);
```

## `console.trace()`

Shows the current call stack.

```js
function first() {
    second();
}

function second() {
    console.trace();
}

first();
```

This helps understand which functions led to the current point.

---

# 10. A Simple Debugging Example

Suppose:

```js
function calculateTotal(price, quantity) {
    return price + quantity;
}

console.log(calculateTotal(100, 3));
```

Output:

```text
103
```

But we expected `300`.

There is no runtime error, but the logic is wrong.

Debug it:

```js
function calculateTotal(price, quantity) {
    console.log("price:", price);
    console.log("quantity:", quantity);

    return price * quantity;
}
```

Now the problem becomes easier to identify.

### Important

Not every bug produces an error.

```text
Syntax bug    → invalid code
Runtime error → program fails while running
Logic bug     → program runs but gives wrong result
```

---

# 11. Useful Debugging Strategies

### 1. Read the error first

Do not ignore the error message.

### 2. Go to the reported line

Check the exact line and nearby code.

### 3. Check the values

Use:

```js
console.log(value);
```

### 4. Trace the data flow

Ask:

```text
Where did this value come from?
Was it changed?
Is its type correct?
```

### 5. Find the root cause

The first visible error is not always the original problem.

### 6. Make one change at a time

Then run the program again.

### 7. Test edge cases

Examples:

```text
empty array
missing value
null
undefined
0
negative number
wrong data type
```

---

# 12. Daily Stack-Trace Practice

For practice, intentionally create small errors and read their stack traces.

Example:

```js
function divide(a, b) {
    return a / b;
}

function calculate() {
    return divide(10, "hello");
}

calculate();
```

Then create other examples involving:
- `ReferenceError`
- `TypeError`
- `SyntaxError`
- nested function calls

The goal is to become comfortable reading:

```text
error type
→ message
→ file
→ line/column
→ call stack
→ root cause
```

Doing this regularly makes debugging much faster.

---

# 13. Quick Revision Notes

```text
Debugging = finding and fixing bugs.

SyntaxError   → invalid JavaScript syntax
ReferenceError → identifier cannot be found
TypeError     → operation is invalid for the value/type

throw         → deliberately raise an error
new Error()   → creates a standard Error object

try           → code that may fail
catch         → handles the error
finally       → runs after try/catch

error.name
error.message
error.stack

console.log()
console.error()
console.warn()
console.info()
console.table()
console.dir()
console.trace()
```

Remember:

```text
throw
 ↓
skip remaining try code
 ↓
catch
 ↓
continue after try/catch
```

# 14. MountBlue Review Questions

1. What is debugging?
2. What is a bug?
3. What is a `SyntaxError`?
4. What is a `ReferenceError`?
5. What is a `TypeError`?
6. How do you read a stack trace?
7. What information does a stack trace provide?
8. What does `throw` do?
9. Why is `throw new Error()` preferred over throwing a string?
10. What are `error.name`, `error.message`, and `error.stack`?
11. What is `try...catch` used for?
12. What happens to the remaining code inside `try` after `throw`?
13. Does execution continue after the `catch` block?
14. What is `finally` used for?
15. Difference between `console.log()` and `console.error()`?
16. What is `console.table()` useful for?
17. What does `console.trace()` show?
18. What is a logic bug?
19. Does every bug produce a runtime error?
20. How would you debug a function that returns the wrong result?
