# Code Quality and JavaScript Background

## 1. What is Code Quality?

Code quality means writing code that is:

- Easy to read
- Easy to understand
- Easy to test
- Easy to maintain
- Less likely to contain bugs

Code should not only work. Other developers should also be able to understand and change it easily.

---

# 2. Good Indentation

Use consistent indentation.

Good:

```js
function calculateTotal(price, quantity) {
    const total = price * quantity;
    return total;
}
```

Bad:

```js
function calculateTotal(price,quantity){
const total=price*quantity;
return total;
}
```

Good formatting makes the structure of the code clear.

---

# 3. Meaningful Names

Variable and function names should explain what they represent.

Good:

```js
const studentName = "Adi";
const totalMarks = 450;

function calculateAverage(marks) {
    // ...
}
```

Avoid unclear names:

```js
const x = "Adi";
const a = 450;

function doSomething(data) {
    // ...
}
```

A good name reduces the need for comments explaining obvious code.

---

# 4. Avoid Unnecessary Global Variables

Avoid putting everything in global scope.

Instead of:

```js
let total = 0;

function add(price) {
    total += price;
}
```

prefer keeping related data inside the function when possible:

```js
function calculateTotal(prices) {
    let total = 0;

    for (const price of prices) {
        total += price;
    }

    return total;
}
```

This makes the function easier to understand and reduces unexpected changes from other parts of the program.

---

# 5. Avoid Duplicate Code

If the same logic appears many times, consider creating a function.

Instead of:

```js
const total1 = price1 * quantity1;
const total2 = price2 * quantity2;
const total3 = price3 * quantity3;
```

we can write:

```js
function calculateTotal(price, quantity) {
    return price * quantity;
}

const total1 = calculateTotal(price1, quantity1);
const total2 = calculateTotal(price2, quantity2);
const total3 = calculateTotal(price3, quantity3);
```

This is easier to maintain.

If the calculation changes, we only need to update one function.

---

# 6. Keep Functions Small and Focused

A function should ideally have one clear responsibility.

Good:

```js
function calculateTotal(price, quantity) {
    return price * quantity;
}

function printTotal(total) {
    console.log(`Total: ${total}`);
}
```

Instead of making one large function handle unrelated tasks such as:

```text
read data
validate data
calculate values
format output
save data
```

Separate responsibilities when it improves clarity.

---

# 7. Prefer Clear Code Over Clever Code

Shorter code is not always better.

Hard to understand:

```js
const result = users.filter(u => u.a > 18).map(u => u.n);
```

Clearer:

```js
const adults = users.filter(user => user.age > 18);
const adultNames = adults.map(user => user.name);
```

The second version is easier to read and debug.

---

# 8. Avoid Unnecessary Comments

Comments are useful when they explain **why** something is done.

Useful:

```js
// Ignore cancelled orders because they should not affect revenue.
const validOrders = orders.filter(order => !order.cancelled);
```

Not very useful:

```js
// Add two numbers
const total = a + b;
```

The code already explains what it does.

---

# 9. Basic JavaScript Best Practices

```text
Use meaningful names.
Keep code consistently formatted.
Prefer const when reassignment is not needed.
Use let when reassignment is needed.
Avoid unnecessary global variables.
Avoid duplicate logic.
Keep functions focused.
Prefer readable code over clever code.
Use comments mainly to explain why.
Test edge cases.
```

---

# 10. What is JavaScript?

JavaScript is a programming language commonly used to make web pages interactive.

It is also used outside the browser, for example with **Node.js**.

JavaScript can be used for:

```text
Web pages
Server-side applications
APIs
Command-line programs
Automation
```

---

# 11. JavaScript History

JavaScript was created by **Brendan Eich** in 1995.

It was initially developed for the Netscape browser.

JavaScript was later standardized under the name **ECMAScript**.

Today, JavaScript continues to receive new language features through ECMAScript specifications.

### JavaScript vs ECMAScript

```text
ECMAScript → language specification/standard
JavaScript  → implementation of that language
```

In everyday programming, people normally call the language JavaScript.

---

# 12. What is ECMAScript?

ECMAScript is the standard that defines the language features of JavaScript.

For example, features such as:

```text
let / const
arrow functions
classes
modules
destructuring
spread/rest
```

were introduced through ECMAScript editions.

JavaScript engines implement these language features.

---

# 13. Imperative vs Declarative Programming

## Imperative Programming

Imperative code explains **how** to perform a task.

Example:

```js
const numbers = [1, 2, 3, 4];
const evenNumbers = [];

for (const number of numbers) {
    if (number % 2 === 0) {
        evenNumbers.push(number);
    }
}
```

We explicitly describe the steps.

---

## Declarative Programming

Declarative code focuses more on **what** result we want.

```js
const numbers = [1, 2, 3, 4];

const evenNumbers = numbers.filter(number => number % 2 === 0);
```

We describe what we want: numbers that satisfy the condition.

### Easy way to remember

```text
Imperative  → HOW
Declarative → WHAT
```

JavaScript supports both styles.

---

# 14. How to Search JavaScript Documentation

When you do not know a method or feature, learn to search for it instead of guessing.

For example, if you want to know how `Array.prototype.filter()` works, search:

```text
JavaScript Array filter MDN
```

Good documentation usually tells you:

```text
What it does
Syntax
Parameters
Return value
Examples
Exceptions/edge cases
Browser/runtime support
```

**MDN Web Docs** is one of the most useful references for JavaScript.

When reading documentation, first understand:

1. What problem the feature solves.
2. What arguments it accepts.
3. What it returns.
4. Whether it changes the original value.
5. A small example.

---

# 15. Practical Code-Quality Example

Less readable:

```js
function f(a){
let x=0;
for(let i of a){if(i>10)x+=i}
return x
}
```

More readable:

```js
function sumNumbersAboveTen(numbers) {
    let total = 0;

    for (const number of numbers) {
        if (number > 10) {
            total += number;
        }
    }

    return total;
}
```

Both can produce the same result, but the second version makes the purpose obvious.

---

# 16. Quick Revision Notes

```text
Good code:
→ readable
→ maintainable
→ testable
→ consistent

Use:
→ meaningful names
→ proper indentation
→ small focused functions
→ reusable functions
→ minimal useful comments

Avoid:
→ unnecessary globals
→ duplicate code
→ unclear names
→ overly clever code

JavaScript:
→ programming language
→ created by Brendan Eich in 1995

ECMAScript:
→ standard/specification behind JavaScript

Imperative → HOW
Declarative → WHAT

Documentation:
→ understand purpose
→ check parameters
→ check return value
→ check mutation/side effects
→ read examples
```