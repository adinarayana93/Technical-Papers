# Modern JavaScript

Modern JavaScript added shorter and cleaner ways to work with arrays, objects, functions, and strings.

The main concepts here are:

- Spread operator
- Rest parameters
- Template literals
- Default parameters
- Array destructuring
- Object destructuring
- Destructuring function parameters

---

# 1. Spread Operator (`...`)

The **spread operator** expands the elements of an array or properties of an object.

## Spread with Arrays

```js
const first = [1, 2, 3];
const second = [4, 5, 6];

const numbers = [...first, ...second];

console.log(numbers);
```

Output:

```text
[1, 2, 3, 4, 5, 6]
```

It is useful for combining or copying arrays.

```js
const original = [10, 20, 30];
const copy = [...original];

console.log(copy);
```

`copy` is a new array.

---

## Spread with Objects

```js
const user = {
    name: "Adi",
    age: 25
};

const updatedUser = {
    ...user,
    city: "Mumbai"
};

console.log(updatedUser);
```

Output:

```text
{
    name: "Adi",
    age: 25,
    city: "Mumbai"
}
```

If the same property appears later, the later value wins.

```js
const user = {
    name: "Adi",
    age: 25
};

const updatedUser = {
    ...user,
    age: 26
};

console.log(updatedUser.age);
```

Output:

```text
26
```

---

# 2. Rest Parameters (`...`)

Rest parameters collect multiple arguments into an array.

```js
function addAll(...numbers) {
    return numbers.reduce((total, number) => total + number, 0);
}

console.log(addAll(10, 20, 30));
```

Output:

```text
60
```

Here:

```js
...numbers
```

collects:

```js
[10, 20, 30]
```

### Spread vs Rest

They use the same `...` syntax, but their purpose is different.

```text
Spread → expands values
Rest   → collects values
```

Example:

```js
const numbers = [1, 2, 3];

console.log(...numbers); // spread
```

```js
function show(...numbers) { // rest
    console.log(numbers);
}
```

---

# 3. Template Literals

Template literals use backticks:

```js
const name = "Adi";

console.log(`Hello ${name}`);
```

Output:

```text
Hello Adi
```

`${...}` is used to insert an expression.

```js
const price = 100;
const quantity = 3;

console.log(`Total: ${price * quantity}`);
```

Output:

```text
Total: 300
```

They are easier to read than string concatenation.

Instead of:

```js
console.log("Hello " + name + ", your age is " + age);
```

we can write:

```js
console.log(`Hello ${name}, your age is ${age}`);
```

Template literals can also contain multiple lines:

```js
const message = `Hello Adi,
Welcome to JavaScript.`;

console.log(message);
```

---

# 4. Default Parameters

Default parameters provide a value when an argument is missing or `undefined`.

```js
function greet(name = "Guest") {
    console.log(`Hello ${name}`);
}

greet();
greet("Adi");
```

Output:

```text
Hello Guest
Hello Adi
```

The default is used for `undefined`:

```js
greet(undefined);
```

It is not used for other values such as `null`.

```js
function greet(name = "Guest") {
    console.log(name);
}

greet(null);
```

Output:

```text
null
```

---

# 5. Array Destructuring

Destructuring allows us to take values from an array and store them in variables.

Without destructuring:

```js
const numbers = [10, 20, 30];

const first = numbers[0];
const second = numbers[1];
```

With destructuring:

```js
const numbers = [10, 20, 30];

const [first, second] = numbers;

console.log(first);
console.log(second);
```

Output:

```text
10
20
```

The positions matter.

```js
const [first, , third] = [10, 20, 30];

console.log(first, third);
```

Output:

```text
10 30
```

The second value was skipped.

---

## Array Destructuring with Default Values

```js
const numbers = [10];

const [first, second = 20] = numbers;

console.log(first, second);
```

Output:

```text
10 20
```

---

# 6. Object Destructuring

Object destructuring extracts properties into variables.

Without destructuring:

```js
const user = {
    name: "Adi",
    age: 25
};

const name = user.name;
const age = user.age;
```

With destructuring:

```js
const user = {
    name: "Adi",
    age: 25
};

const { name, age } = user;

console.log(name);
console.log(age);
```

Output:

```text
Adi
25
```

Unlike arrays, object destructuring uses **property names**, not positions.

---

## Rename a Destructured Property

```js
const user = {
    name: "Adi",
    age: 25
};

const { name: userName } = user;

console.log(userName);
```

Output:

```text
Adi
```

Here:

```text
name      → object property
userName  → new variable name
```

---

## Object Destructuring with Default Values

```js
const user = {
    name: "Adi"
};

const { name, age = 25 } = user;

console.log(name, age);
```

Output:

```text
Adi 25
```

---

# 7. Destructuring Function Parameters

We can destructure an object directly in a function parameter.

Without destructuring:

```js
function greet(user) {
    console.log(`Hello ${user.name}`);
}

greet({ name: "Adi" });
```

With destructuring:

```js
function greet({ name }) {
    console.log(`Hello ${name}`);
}

greet({ name: "Adi" });
```

Output:

```text
Hello Adi
```

This is very useful when a function needs only a few properties from a larger object.

Example:

```js
function printStudent({ name, marks }) {
    console.log(`${name}: ${marks}`);
}

printStudent({
    name: "Adi",
    marks: 85,
    city: "Mumbai"
});
```

Output:

```text
Adi: 85
```

The function receives the complete object but extracts only what it needs.

---

# 8. Combining These Features

Modern JavaScript features are often used together.

Example:

```js
const user = {
    name: "Adi",
    age: 25
};

function createMessage({ name, age }) {
    return `Hello ${name}, you are ${age} years old.`;
}

console.log(createMessage(user));
```

Output:

```text
Hello Adi, you are 25 years old.
```

Another example using spread:

```js
const user = {
    name: "Adi",
    age: 25
};

const updatedUser = {
    ...user,
    age: 26
};

console.log(updatedUser);
```

---

# 9. Quick Revision Notes

```text
Spread (...)       → expands array/object values
Rest (...)         → collects function arguments

Template literals  → `Hello ${name}`
Default parameters → parameter fallback value

Array destructuring
const [a, b] = array;

Object destructuring
const { name, age } = user;

Rename property
const { name: userName } = user;

Function parameter destructuring
function greet({ name }) {
    ...
}
```

Remember:

```text
Spread → take values OUT
Rest   → gather values IN
```

---

# 10. When to Use Them

| Feature | Useful when |
|---|---|
| Spread | Copying/combining arrays or objects |
| Rest | Accepting an unknown number of arguments |
| Template literals | Building strings with variables |
| Default parameters | Providing fallback parameter values |
| Array destructuring | Extracting array values by position |
| Object destructuring | Extracting object properties |
| Parameter destructuring | Functions need selected object properties |

---

# 11. MountBlue Review Questions

1. What is the spread operator?
2. What is the rest parameter?
3. Difference between spread and rest?
4. How do you copy an array using spread?
5. How do you combine two arrays using spread?
6. How do you copy/extend an object using spread?
7. What happens when the same object property is written twice with spread?
8. What are template literals?
9. Why are template literals useful?
10. What are default parameters?
11. When is a default parameter value used?
12. What is array destructuring?
13. How is object destructuring different from array destructuring?
14. How do you rename a property while destructuring?
15. How do you provide a default value while destructuring?
16. What is destructuring in function parameters?
17. Write a function that accepts `{ name, age }` using parameter destructuring.
18. Explain the difference between these two:
    ```js
    const copy = [...numbers];
    ```
    and
    ```js
    function test(...numbers) {}
    ```
