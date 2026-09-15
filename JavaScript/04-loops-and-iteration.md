# Loops and Iteration in JavaScript

## 1. What is a Loop?

A **loop** repeats a block of code while a condition is true or for each item in a collection.

Instead of writing:

```js
console.log(1);
console.log(2);
console.log(3);
```

we can use:

```js
for (let number = 1; number <= 3; number++) {
    console.log(number);
}
```

Output:

```text
1
2
3
```

Loops are useful when we need to process many values.

---

# 2. `for` Loop

A `for` loop is useful when we know the number of iterations or need an index.

```js
for (let i = 0; i < 5; i++) {
    console.log(i);
}
```

Output:

```text
0
1
2
3
4
```

### Three parts

```js
for (initialization; condition; update) {
    // code
}
```

Example:

```js
for (let i = 0; i < 3; i++) {
    console.log(i);
}
```

- `let i = 0` → starts the loop
- `i < 3` → checks whether to continue
- `i++` → changes `i` after each iteration

---

## Looping through an array with `for`

```js
const numbers = [10, 20, 30];

for (let i = 0; i < numbers.length; i++) {
    console.log(numbers[i]);
}
```

Output:

```text
10
20
30
```

Use this when you need the **index** or more control over the loop.

---

# 3. `while` Loop

A `while` loop runs as long as its condition is true.

```js
let number = 1;

while (number <= 3) {
    console.log(number);
    number++;
}
```

Output:

```text
1
2
3
```

### Important

Make sure the condition can eventually become false.

Bad:

```js
let number = 1;

while (number <= 3) {
    console.log(number);
}
```

`number` never changes, so the loop becomes infinite.

---

# 4. `for...of`

`for...of` is mainly used to get the **values** from an iterable such as an array or string.

```js
const numbers = [10, 20, 30];

for (const number of numbers) {
    console.log(number);
}
```

Output:

```text
10
20
30
```

For a string:

```js
for (const character of "Hello") {
    console.log(character);
}
```

Output:

```text
H
e
l
l
o
```

### `for...of` vs normal `for`

Use:

```js
for (const number of numbers)
```

when you only need the values.

Use:

```js
for (let i = 0; i < numbers.length; i++)
```

when you need indexes or more control.

---

# 5. `for...in`

`for...in` iterates over **property keys** of an object.

```js
const user = {
    name: "Adi",
    age: 25
};

for (const key in user) {
    console.log(key);
}
```

Output:

```text
name
age
```

To get values:

```js
for (const key in user) {
    console.log(user[key]);
}
```

Output:

```text
Adi
25
```

### Important

Do not normally use `for...in` for arrays when you want array values.

For arrays, prefer:

```js
for (const value of numbers) {
    console.log(value);
}
```

---

# 6. `forEach()`

`forEach()` executes a callback once for each array element.

```js
const numbers = [10, 20, 30];

numbers.forEach(function(number) {
    console.log(number);
});
```

Output:

```text
10
20
30
```

Using an arrow function:

```js
numbers.forEach(number => {
    console.log(number);
});
```

It can also provide the index:

```js
numbers.forEach((number, index) => {
    console.log(index, number);
});
```

Output:

```text
0 10
1 20
2 30
```

### `forEach()` vs `for...of`

Both can process array values.

Use `forEach()` when you simply want to perform an action for every element.

Use `for...of` when you may need `break`, `continue`, or more traditional loop control.

---

# 7. `break`

`break` immediately stops the loop.

```js
for (let number = 1; number <= 5; number++) {
    if (number === 3) {
        break;
    }

    console.log(number);
}
```

Output:

```text
1
2
```

Use `break` when you have found what you need or want to stop processing.

---

# 8. `continue`

`continue` skips the current iteration and moves to the next one.

```js
for (let number = 1; number <= 5; number++) {
    if (number === 3) {
        continue;
    }

    console.log(number);
}
```

Output:

```text
1
2
4
5
```

Use `continue` when one particular item should be skipped.

---

# 9. Arrays vs Objects

A simple rule:

```text
Array → collection of values → for...of / forEach
Object → collection of key-value pairs → for...in
```

Example:

```js
const fruits = ["Apple", "Banana"];

for (const fruit of fruits) {
    console.log(fruit);
}
```

Object:

```js
const person = {
    name: "Adi",
    age: 25
};

for (const key in person) {
    console.log(key, person[key]);
}
```

---

# 10. Choosing the Right Loop

| Situation | Good choice |
|---|---|
| Need index/control | `for` |
| Repeat while condition is true | `while` |
| Array/string values | `for...of` |
| Object keys | `for...in` |
| Simple action for every array item | `forEach()` |
| Stop loop immediately | `break` |
| Skip current iteration | `continue` |

### Practical examples

Find the first matching item:

```js
for (const user of users) {
    if (user.id === 5) {
        console.log(user);
        break;
    }
}
```

Process only valid items:

```js
for (const user of users) {
    if (!user.active) {
        continue;
    }

    console.log(user.name);
}
```

---

# 11. Loop Variable Naming

Use names that describe what the variable represents.

Good:

```js
for (const student of students) {
    console.log(student);
}
```

Less clear:

```js
for (const x of students) {
    console.log(x);
}
```

For numeric loops, `i`, `j`, and `k` are commonly used for indexes.

---

# 12. Quick Revision Notes

```text
Loop = repeat code.

for       → useful with indexes/counts
while     → repeat while condition is true
for...of  → values of arrays/strings
for...in  → keys of objects
forEach   → callback for every array element

break     → stop the loop
continue  → skip current iteration

Array  → usually for...of / forEach
Object → usually for...in

Always make sure a while loop can eventually stop.
Use meaningful loop variable names.
```

