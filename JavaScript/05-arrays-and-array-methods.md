# Arrays and Array Methods in JavaScript

## 1. What is an Array?

An **array** stores multiple values in one variable.

```js
const fruits = ["Apple", "Banana", "Mango"];

console.log(fruits[0]);
```

Output:

```text
Apple
```

Array indexes start from `0`.

```text
Apple   → index 0
Banana  → index 1
Mango   → index 2
```

The number of elements is available through `length`.

```js
console.log(fruits.length);
```

Output:

```text
3
```

---

# 2. Adding and Removing Elements

## `push()`

Adds one or more elements to the **end**.

```js
const numbers = [10, 20];

numbers.push(30);

console.log(numbers);
```

Output:

```text
[10, 20, 30]
```

`push()` changes the original array.

---

## `pop()`

Removes the **last** element and returns it.

```js
const numbers = [10, 20, 30];

const removed = numbers.pop();

console.log(removed);
console.log(numbers);
```

Output:

```text
30
[10, 20]
```

---

# 3. Combining Arrays

## `concat()`

Combines arrays and returns a **new array**.

```js
const first = [1, 2];
const second = [3, 4];

const result = first.concat(second);

console.log(result);
```

Output:

```text
[1, 2, 3, 4]
```

The original arrays are unchanged.

---

# 4. Getting Part of an Array

## `slice()`

Returns a portion of an array without changing the original array.

```js
const numbers = [10, 20, 30, 40, 50];

const result = numbers.slice(1, 4);

console.log(result);
```

Output:

```text
[20, 30, 40]
```

The ending index is **not included**.

```text
slice(start, end)
             ↑
          excluded
```

---

## `splice()`

Adds, removes, or replaces elements and **changes the original array**.

```js
const numbers = [10, 20, 30, 40];

numbers.splice(1, 2);

console.log(numbers);
```

Output:

```text
[10, 40]
```

`splice(start, deleteCount, ...items)`

Example:

```js
const numbers = [10, 20, 40];

numbers.splice(2, 0, 30);

console.log(numbers);
```

Output:

```text
[10, 20, 30, 40]
```

### `slice()` vs `splice()`

```text
slice  → returns a portion → does not change original
splice → modifies array   → changes original
```

---

# 5. Converting an Array to a String

## `join()`

Combines array elements into a string.

```js
const fruits = ["Apple", "Banana", "Mango"];

console.log(fruits.join(", "));
```

Output:

```text
Apple, Banana, Mango
```

---

# 6. Flattening Arrays

## `flat()`

Removes nested array levels.

```js
const numbers = [1, [2, 3], [4, [5, 6]]];

console.log(numbers.flat());
```

Output:

```text
[1, 2, 3, 4, [5, 6]]
```

By default, `flat()` removes one level.

To flatten deeper:

```js
console.log(numbers.flat(2));
```

Output:

```text
[1, 2, 3, 4, 5, 6]
```

For any depth:

```js
numbers.flat(Infinity);
```

---

# 7. Finding Elements

## `find()`

Returns the **first element** that satisfies a condition.

```js
const numbers = [5, 12, 8, 20];

const result = numbers.find(number => number > 10);

console.log(result);
```

Output:

```text
12
```

If nothing matches:

```js
console.log(numbers.find(number => number > 100));
```

Output:

```text
undefined
```

Use `find()` when you need the actual first matching element.

---

## `findIndex()`

Returns the index of the first matching element.

```js
const numbers = [5, 12, 8, 20];

console.log(numbers.findIndex(number => number > 10));
```

Output:

```text
1
```

If nothing matches:

```text
-1
```

---

## `indexOf()`

Finds the index of a specific value.

```js
const fruits = ["Apple", "Banana", "Mango"];

console.log(fruits.indexOf("Banana"));
```

Output:

```text
1
```

If the value does not exist:

```text
-1
```

`indexOf()` checks for a specific value, while `findIndex()` uses a condition.

---

## `includes()`

Checks whether an array contains a value.

```js
const fruits = ["Apple", "Banana", "Mango"];

console.log(fruits.includes("Banana"));
```

Output:

```text
true
```

Use it when you only need `true` or `false`.

---

# 8. `forEach()`

`forEach()` runs a callback for every element.

```js
const numbers = [10, 20, 30];

numbers.forEach(number => {
    console.log(number);
});
```

Output:

```text
10
20
30
```

It is mainly used when you want to **perform an action** for each element.

---

# 9. `filter()`

`filter()` creates a **new array** containing elements that pass a condition.

```js
const numbers = [10, 15, 20, 25];

const result = numbers.filter(number => number > 15);

console.log(result);
```

Output:

```text
[20, 25]
```

Use `filter()` when you want **multiple matching elements**.

---

# 10. `map()`

`map()` creates a new array by transforming every element.

```js
const numbers = [1, 2, 3];

const result = numbers.map(number => number * 2);

console.log(result);
```

Output:

```text
[2, 4, 6]
```

Use `map()` when you want **one output for each input element**.

Example:

```js
const names = ["adi", "rahul"];

const upperNames = names.map(name => name.toUpperCase());

console.log(upperNames);
```

Output:

```text
["ADI", "RAHUL"]
```

---

# 11. `reduce()`

`reduce()` processes an array and produces one final result.

```js
const numbers = [10, 20, 30];

const total = numbers.reduce((sum, number) => {
    return sum + number;
}, 0);

console.log(total);
```

Output:

```text
60
```

Here:

```text
sum     → accumulator
number  → current element
0       → initial value
```

Another example:

```js
const numbers = [2, 3, 4];

const product = numbers.reduce((result, number) => {
    return result * number;
}, 1);

console.log(product);
```

Output:

```text
24
```

Use `reduce()` for totals, products, grouping, counting, and other calculations where many values become one result.

---

# 12. `sort()`

`sort()` sorts an array and **changes the original array**.

For numbers, provide a comparison function.

```js
const numbers = [10, 2, 30, 5];

numbers.sort((a, b) => a - b);

console.log(numbers);
```

Output:

```text
[2, 5, 10, 30]
```

Descending:

```js
numbers.sort((a, b) => b - a);
```

Without a comparison function, values are sorted as strings, which can give unexpected results for numbers.

---

# 13. Mutable vs Non-Mutable Methods

### Usually change the original array

```text
push()
pop()
splice()
sort()
```

### Return a new result without changing the original

```text
concat()
slice()
map()
filter()
flat()
```

`forEach()` does not create a transformed array.

Knowing whether a method mutates the array is important for avoiding unexpected changes.

---

# 14. Method Chaining

Array methods can be combined.

Example:

```js
const numbers = [1, 2, 3, 4, 5, 6];

const result = numbers
    .filter(number => number % 2 === 0)
    .map(number => number * 10);

console.log(result);
```

Output:

```text
[20, 40, 60]
```

Process:

```text
[1,2,3,4,5,6]
      ↓ filter
[2,4,6]
      ↓ map
[20,40,60]
```

Chaining is useful when several transformations need to happen in sequence.

---

# 15. Which Method Should I Use?

| Requirement | Method |
|---|---|
| Add to end | `push()` |
| Remove last | `pop()` |
| Combine arrays | `concat()` |
| Get a portion | `slice()` |
| Insert/remove elements | `splice()` |
| Convert array to string | `join()` |
| Flatten nested arrays | `flat()` |
| First matching element | `find()` |
| Index of matching element | `findIndex()` |
| Index of specific value | `indexOf()` |
| Check if value exists | `includes()` |
| Do something for every element | `forEach()` |
| Get matching elements | `filter()` |
| Transform every element | `map()` |
| Produce one final result | `reduce()` |
| Sort elements | `sort()` |

---

# 16. Quick Revision Notes

```text
push/pop     → add/remove from end
slice        → copy a portion
splice       → modify array
concat       → combine arrays
join         → array → string
flat         → remove nested levels

find         → first matching element
findIndex    → index of first match
indexOf      → index of a specific value
includes     → true/false for a specific value

forEach      → perform an action
filter       → keep matching elements
map          → transform every element
reduce       → many values → one result
sort         → arrange elements
```

Remember:

```text
find   → one element
filter → many elements

map    → transform
reduce → combine

slice  → does not modify original
splice → modifies original
```

# 17. MountBlue Review Questions

1. What is an array?
2. Do array indexes start at 0 or 1?
3. What does `push()` return?
4. What does `pop()` do?
5. Difference between `slice()` and `splice()`?
6. What does `concat()` do?
7. What does `join()` return?
8. What does `flat()` do?
9. Difference between `find()` and `filter()`?
10. Difference between `findIndex()` and `indexOf()`?
11. What does `includes()` return?
12. Difference between `forEach()` and `map()`?
13. When would you use `reduce()`?
14. Why do we use `(a, b) => a - b` with numeric `sort()`?
15. Which common array methods mutate the original array?
16. What is method chaining?
17. Given `[1, 2, 3, 4]`, write code to get `[2, 4]`.
18. Given `[1, 2, 3]`, write code to get `[10, 20, 30]`.
19. Given `[10, 20, 30]`, use `reduce()` to calculate the total.
