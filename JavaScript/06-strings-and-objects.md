# Strings and Objects in JavaScript

## 1. Strings

A **string** is text written inside quotes.

```js
const name = "Adi";
const city = 'Mumbai';
```

Strings are **immutable**. This means string methods return a new string instead of changing the original string.

```js
const name = "adi";

const result = name.toUpperCase();

console.log(result);
console.log(name);
```

Output:

```text
ADI
adi
```

---

# 2. Common String Methods

## `length`

Returns the number of characters.

```js
const name = "JavaScript";

console.log(name.length);
```

Output:

```text
10
```

---

## `toUpperCase()` / `toLowerCase()`

Changes the case in the returned string.

```js
const text = "Hello World";

console.log(text.toUpperCase());
console.log(text.toLowerCase());
```

Output:

```text
HELLO WORLD
hello world
```

---

## `trim()`

Removes whitespace from the beginning and end.

```js
const text = "   Hello   ";

console.log(text.trim());
```

Output:

```text
Hello
```

Useful when processing user input.

---

## `includes()`

Checks whether a string contains some text.

```js
const text = "JavaScript";

console.log(text.includes("Script"));
```

Output:

```text
true
```

---

## `indexOf()`

Returns the position of the first occurrence.

```js
const text = "JavaScript";

console.log(text.indexOf("Script"));
```

Output:

```text
4
```

If it is not found:

```text
-1
```

---

## `startsWith()` / `endsWith()`

Checks the beginning or end of a string.

```js
const file = "report.pdf";

console.log(file.startsWith("report"));
console.log(file.endsWith(".pdf"));
```

Output:

```text
true
true
```

---

## `slice()`

Extracts part of a string.

```js
const text = "JavaScript";

console.log(text.slice(0, 4));
```

Output:

```text
Java
```

The ending index is excluded.

Negative indexes can be used:

```js
console.log(text.slice(-6));
```

Output:

```text
Script
```

---

## `substring()`

Also extracts part of a string.

```js
const text = "JavaScript";

console.log(text.substring(0, 4));
```

Output:

```text
Java
```

For normal positive indexes, `slice()` and `substring()` can look similar.

Important difference: `slice()` supports negative indexes, while `substring()` treats negative values as `0`.

---

## `replace()`

Replaces the first matching occurrence.

```js
const text = "cat cat";

console.log(text.replace("cat", "dog"));
```

Output:

```text
dog cat
```

---

## `replaceAll()`

Replaces all matching occurrences.

```js
const text = "cat cat";

console.log(text.replaceAll("cat", "dog"));
```

Output:

```text
dog dog
```

---

## `split()`

Splits a string into an array.

```js
const text = "Apple,Banana,Mango";

const fruits = text.split(",");

console.log(fruits);
```

Output:

```text
["Apple", "Banana", "Mango"]
```

Very useful when converting text into an array of values.

---

## `charAt()`

Returns the character at a given index.

```js
const text = "Hello";

console.log(text.charAt(1));
```

Output:

```text
e
```

---

# 3. String Method Quick Guide

```text
length          → number of characters
toUpperCase()   → uppercase
toLowerCase()   → lowercase
trim()          → remove outside whitespace
includes()      → check for text
indexOf()       → find position
startsWith()    → check beginning
endsWith()      → check ending
slice()         → extract part
substring()     → extract part
replace()       → replace first match
replaceAll()    → replace all matches
split()         → string → array
charAt()        → get character
```

---

# 4. Objects

An **object** stores data as key-value pairs.

```js
const user = {
    name: "Adi",
    age: 25,
    city: "Mumbai"
};
```

Here:

```text
name → "Adi"
age  → 25
city → "Mumbai"
```

---

## Accessing Properties

### Dot notation

```js
console.log(user.name);
```

Output:

```text
Adi
```

### Bracket notation

```js
console.log(user["name"]);
```

Output:

```text
Adi
```

Bracket notation is especially useful when the property name is stored in a variable.

```js
const key = "city";

console.log(user[key]);
```

Output:

```text
Mumbai
```

---

# 5. Adding, Updating, and Deleting Properties

### Add

```js
user.email = "adi@example.com";
```

### Update

```js
user.age = 26;
```

### Delete

```js
delete user.city;
```

Objects are mutable, so these operations change the original object.

---

# 6. `Object.keys()`

Returns an array containing the object's keys.

```js
const user = {
    name: "Adi",
    age: 25
};

console.log(Object.keys(user));
```

Output:

```text
["name", "age"]
```

---

# 7. `Object.values()`

Returns an array containing the object's values.

```js
console.log(Object.values(user));
```

Output:

```text
["Adi", 25]
```

---

# 8. `Object.entries()`

Returns key-value pairs as nested arrays.

```js
console.log(Object.entries(user));
```

Output:

```text
[["name", "Adi"], ["age", 25]]
```

This is useful when you need both the key and value.

Example:

```js
for (const [key, value] of Object.entries(user)) {
    console.log(key, value);
}
```

---

# 9. `Object.assign()`

Copies properties from one or more objects into a target object.

```js
const user = {
    name: "Adi"
};

const extra = {
    age: 25
};

const result = Object.assign({}, user, extra);

console.log(result);
```

Output:

```text
{ name: "Adi", age: 25 }
```

Using `{}` as the first argument creates a new target object, so `user` is not changed.

---

# 10. `Object.hasOwn()`

Checks whether an object directly contains a property.

```js
const user = {
    name: "Adi",
    age: 25
};

console.log(Object.hasOwn(user, "name"));
console.log(Object.hasOwn(user, "email"));
```

Output:

```text
true
false
```

This is useful when you need to distinguish an object's own property from inherited properties.

---

# 11. `Object.fromEntries()`

Converts key-value pairs into an object.

```js
const entries = [
    ["name", "Adi"],
    ["age", 25]
];

const user = Object.fromEntries(entries);

console.log(user);
```

Output:

```text
{ name: "Adi", age: 25 }
```

It is essentially the reverse direction of `Object.entries()`.

```text
Object.entries(object)
        ↓
[["name", "Adi"], ["age", 25]]

Object.fromEntries(entries)
        ↓
{name: "Adi"}
```

---

# 12. Iterating Through Objects

Using `for...in`:

```js
const user = {
    name: "Adi",
    age: 25
};

for (const key in user) {
    console.log(key, user[key]);
}
```

Using `Object.entries()`:

```js
for (const [key, value] of Object.entries(user)) {
    console.log(key, value);
}
```

Both can process key-value pairs.

---

# 13. Strings vs Objects

| Strings | Objects |
|---|---|
| Store text | Store key-value data |
| Immutable | Mutable |
| Access by index | Access by property |
| `text[0]` | `user.name` |
| String methods | Object methods/utilities |

---

# 14. Practical Example

Suppose user input contains:

```js
const input = "  adi@example.com  ";
```

Clean it:

```js
const email = input.trim().toLowerCase();

console.log(email);
```

Output:

```text
adi@example.com
```

Check it:

```js
if (email.endsWith("@example.com")) {
    console.log("Valid domain");
}
```

Output:

```text
Valid domain
```

---

# 15. Quick Revision Notes

```text
Strings are immutable.

trim()        → clean outside spaces
includes()    → check whether text exists
indexOf()     → find position
slice()       → extract text
replace()     → replace first match
replaceAll()  → replace all matches
split()       → convert string to array

Object = key-value pairs.

Object.keys()       → keys
Object.values()     → values
Object.entries()    → [key, value] pairs
Object.assign()     → copy/merge properties
Object.hasOwn()     → check own property
Object.fromEntries() → entries → object
```
