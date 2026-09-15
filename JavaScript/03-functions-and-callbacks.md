# Functions and Callbacks in JavaScript

## 1. What is a Function?

A **function** is a reusable block of code that performs a task.

```js
function greet() {
    console.log("Hello");
}

greet();
```

Output:

```text
Hello
```

Instead of writing the same code many times, we define it once and call it whenever needed.

---

## 2. Function Declaration

```js
function add(a, b) {
    return a + b;
}

console.log(add(10, 20));
```

Output:

```text
30
```

A function declaration can be called before its definition because function declarations are hoisted.

```js
greet();

function greet() {
    console.log("Hello");
}
```

---

## 3. Function Expression

A function can also be stored in a variable.

```js
const add = function(a, b) {
    return a + b;
};

console.log(add(10, 20));
```

Here, the function is an **anonymous function** because it has no name.

A function expression is not callable before its assignment:

```js
// add(10, 20); // ReferenceError

const add = function(a, b) {
    return a + b;
};
```

---

## 4. Named and Anonymous Functions

### Named function

```js
const add = function addNumbers(a, b) {
    return a + b;
};
```

The function itself has the name `addNumbers`.

### Anonymous function

```js
const add = function(a, b) {
    return a + b;
};
```

The function has no internal name.

Anonymous functions are commonly used as callbacks.

---

# 5. Parameters and Arguments

**Parameters** are the variables written in the function definition.

```js
function greet(name) {
    console.log("Hello " + name);
}
```

`name` is a parameter.

**Arguments** are the actual values passed when calling the function.

```js
greet("Adi");
```

`"Adi"` is an argument.

With multiple values:

```js
function add(a, b) {
    return a + b;
}

add(10, 20);
```

`a` and `b` → parameters  
`10` and `20` → arguments

---

# 6. Default Parameters

A default parameter is used when an argument is not provided.

```js
function greet(name = "Guest") {
    console.log("Hello " + name);
}

greet();
greet("Adi");
```

Output:

```text
Hello Guest
Hello Adi
```

---

# 7. Arrow Functions

Arrow functions provide shorter function syntax.

Regular function:

```js
function add(a, b) {
    return a + b;
}
```

Arrow function:

```js
const add = (a, b) => {
    return a + b;
};
```

For a single expression, we can shorten it further:

```js
const add = (a, b) => a + b;
```

For one parameter, parentheses can be omitted:

```js
const square = number => number * number;
```

### Important difference

Regular functions have their own `this`.

Arrow functions do **not** create their own `this`; they use `this` from the surrounding scope.

For normal callbacks and data processing, arrow functions are commonly convenient.

---

# 8. Rest Parameters — Variable Number of Arguments

Rest parameters collect remaining arguments into an array.

```js
function addAll(...numbers) {
    let total = 0;

    for (const number of numbers) {
        total += number;
    }

    return total;
}

console.log(addAll(10, 20, 30));
```

Output:

```text
60
```

`...numbers` becomes:

```js
[10, 20, 30]
```

Rest parameters are useful when the number of arguments is not fixed.

---

# 9. Passing a Function to Another Function

Functions are values in JavaScript.

So we can store a function in a variable and pass it to another function.

```js
function greet(name) {
    console.log("Hello " + name);
}

function execute(fn) {
    fn("Adi");
}

execute(greet);
```

Output:

```text
Hello Adi
```

Notice:

```js
execute(greet);
```

We pass the function itself.

If we write:

```js
execute(greet());
```

we are calling `greet()` first and passing its return value instead.

---

# 10. Callback Function

A **callback** is a function passed to another function so that the other function can call it later.

```js
function processUser(name, callback) {
    callback(name);
}

function greet(name) {
    console.log("Hello " + name);
}

processUser("Adi", greet);
```

Output:

```text
Hello Adi
```

Here:

- `greet` → callback
- `processUser` → receives and calls the callback

Callbacks are heavily used in JavaScript.

---

## Callback with an Anonymous Function

```js
function processUser(name, callback) {
    callback(name);
}

processUser("Adi", function(name) {
    console.log("Hello " + name);
});
```

The callback can also be written as an arrow function:

```js
processUser("Adi", name => {
    console.log("Hello " + name);
});
```

---

# 11. Higher-Order Functions

A **higher-order function** is a function that:

1. takes another function as an argument, or
2. returns a function.

Example:

```js
function calculate(a, b, operation) {
    return operation(a, b);
}

function add(a, b) {
    return a + b;
}

console.log(calculate(10, 20, add));
```

Output:

```text
30
```

`calculate` is a higher-order function because it receives a function.

### Function returning a function

```js
function multiplyBy(number) {
    return function(value) {
        return value * number;
    };
}

const double = multiplyBy(2);

console.log(double(5));
```

Output:

```text
10
```

---

# 12. `return` from a Function

`return` sends a value back to the place where the function was called.

```js
function add(a, b) {
    return a + b;
}

const result = add(10, 20);

console.log(result);
```

Output:

```text
30
```

Without `return`:

```js
function add(a, b) {
    console.log(a + b);
}

const result = add(10, 20);

console.log(result);
```

Output:

```text
30
undefined
```

`console.log()` displays a value.  
`return` gives a value back to the caller.

---

# 13. Pass-by-Value and Objects

JavaScript passes arguments **by value**.

For primitive values:

```js
function change(number) {
    number = 100;
}

let age = 25;

change(age);

console.log(age);
```

Output:

```text
25
```

Changing the parameter does not change the original variable.

With objects, the value being passed is a **reference to the object**.

```js
function changeName(user) {
    user.name = "Rahul";
}

const user = { name: "Adi" };

changeName(user);

console.log(user.name);
```

Output:

```text
Rahul
```

The function received a copy of the reference, so both variables refer to the same object.

But reassigning the parameter does not replace the original object:

```js
function changeUser(user) {
    user = { name: "Rahul" };
}

const user = { name: "Adi" };

changeUser(user);

console.log(user.name);
```

Output:

```text
Adi
```

### Easy way to remember

```text
Primitive:
copy of value → changing parameter does not affect original

Object/Array:
copy of reference → changing object contents can affect original
```

---

# 14. When to Use What?

| Situation | Good choice |
|---|---|
| Reusable named operation | Function declaration |
| Store function in a variable | Function expression |
| Short callback | Arrow function |
| Need `this` from surrounding scope | Arrow function |
| Variable number of arguments | Rest parameter |
| Pass behavior into another function | Callback |
| Function accepts/returns another function | Higher-order function |
| Send result back to caller | `return` |

---

# 15. Quick Revision Notes

```text
Function = reusable block of code.

Parameter = variable in function definition.
Argument = actual value passed during function call.

Function declaration → function greet() {}
Function expression  → const greet = function() {}
Arrow function       → const greet = () => {}

Default parameter → gives a value when argument is missing.
Rest parameter    → collects multiple arguments into an array.

Callback = function passed to another function.
Higher-order function = accepts or returns a function.

return → sends a value back to the caller.
console.log() → only displays a value.

JavaScript passes arguments by value.
For objects/arrays, the copied value is a reference to the same object.
```
