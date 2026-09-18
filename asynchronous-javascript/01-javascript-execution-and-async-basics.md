# JavaScript Execution and Asynchronous JavaScript Basics

## 1. How does JavaScript execute the code?

JavaScript is mainly **single-threaded**. It executes one piece of JavaScript code at a time.

A simple mental model:

```text
JavaScript code
      ↓
   Call Stack
      ↓
Execute one operation at a time
```

Example:

```js
console.log("A");
console.log("B");
console.log("C");
```

Output:

```text
A
B
C
```

### What is the Call Stack?

The **Call Stack** keeps track of functions that are currently being executed.

```js
function greet() {
    console.log("Hello");
}

greet();
```

Simplified execution:

```text
Global code starts
      ↓
greet() is called
      ↓
greet() goes onto Call Stack
      ↓
console.log() executes
      ↓
greet() finishes
      ↓
greet() is removed from Call Stack
```

Think of the Call Stack like a stack of plates:

```text
┌──────────────┐
│   greet()    │ ← currently executing
├──────────────┤
│ global code  │
└──────────────┘
```

The last function added is the first one completed.

---

## 2. What is the difference between synchronous and asynchronous code?

### Synchronous code

Synchronous code waits for the current operation to finish before moving to the next operation.

```js
console.log("Start");
console.log("Middle");
console.log("End");
```

Output:

```text
Start
Middle
End
```

Execution:

```text
Start
  ↓
Middle
  ↓
End
```

### Asynchronous code

Asynchronous code can start an operation and continue with other work instead of waiting for that operation to finish.

```js
console.log("Start");

setTimeout(() => {
    console.log("Timer finished");
}, 2000);

console.log("End");
```

Output:

```text
Start
End
Timer finished
```

JavaScript does not stop the whole program for two seconds.

### Simple difference

| Synchronous | Asynchronous |
|---|---|
| Waits for current operation | Can continue while waiting |
| Step-by-step execution | Work may finish later |
| Simple flow | Useful for I/O and delayed work |
| Can block execution | Helps avoid unnecessary blocking |

**Important:** asynchronous does not mean JavaScript executes two JavaScript statements simultaneously. JavaScript still executes JavaScript code one piece at a time.

---

## 3. What are the ways to make code asynchronous?

Common approaches are:

### 1. Timers

```js
setTimeout(() => {
    console.log("Runs later");
}, 1000);
```

### 2. Callbacks

A function can receive another function that should be called later.

```js
function doWork(callback) {
    setTimeout(() => {
        callback("Work finished");
    }, 1000);
}

doWork((message) => {
    console.log(message);
});
```

### 3. Promises

```js
Promise.resolve("Done")
    .then((value) => {
        console.log(value);
    });
```

### 4. Async/Await

```js
async function main() {
    const result = await Promise.resolve("Done");
    console.log(result);
}

main();
```

`async/await` is built on top of Promises and gives asynchronous code a style that looks more like synchronous code.

---

## 4. What are Web Browser APIs?

JavaScript itself does not provide every feature needed by a web page.

The **browser environment provides APIs** that JavaScript can use.

Examples:

- Timers such as `setTimeout()`
- DOM APIs
- Fetch API
- Browser events
- Geolocation
- Web Storage

Example:

```js
setTimeout(() => {
    console.log("Hello");
}, 1000);
```

The timer is handled by the browser environment. JavaScript does not sit in the Call Stack for one second waiting.

Another example:

```js
fetch("https://example.com/data")
    .then((response) => response.json())
    .then((data) => {
        console.log(data);
    });
```

The browser can handle the network operation while JavaScript continues executing other code.

### Mental model

```text
JavaScript
   │
   ├── Call Stack
   │
   └── Browser APIs
          │
          ├── Timer
          ├── Network
          ├── DOM
          └── Events
```

The exact APIs depend on the environment. Node.js also provides its own APIs, so not every API is a browser API.

---

## 5. What is the Event Loop?

The **Event Loop** helps JavaScript handle asynchronous work without blocking the main JavaScript execution.

A simplified model:

```text
             ┌───────────────┐
             │  Call Stack   │
             └───────┬───────┘
                     │
                     ↓
             ┌───────────────┐
             │   Event Loop  │
             └───────┬───────┘
                     │
          ┌──────────┴──────────┐
          ↓                     ↓
    Task/Callback Queue    Microtask Queue
```

Example:

```js
console.log("Start");

setTimeout(() => {
    console.log("Timer");
}, 0);

console.log("End");
```

Output:

```text
Start
End
Timer
```

Why?

```text
console.log("Start")
        ↓
Start printed

setTimeout(...)
        ↓
Timer callback is registered
        ↓
JavaScript continues

console.log("End")
        ↓
End printed

Call Stack becomes empty
        ↓
Timer callback can run
        ↓
Timer printed
```

`setTimeout(..., 0)` does **not** mean "execute immediately."

It means the callback becomes eligible after the timer has completed and the runtime can process it.

---

## 6. What is callback hell?

A **callback** is a function passed to another function so it can be called later.

Example:

```js
function getUser(callback) {
    setTimeout(() => {
        callback("User data");
    }, 1000);
}

getUser((user) => {
    console.log(user);
});
```

This is fine.

The problem happens when many asynchronous operations depend on one another and callbacks become deeply nested.

```js
getUser((user) => {
    getOrders(user, (orders) => {
        getPayment(orders, (payment) => {
            sendEmail(payment, () => {
                console.log("Email sent");
            });
        });
    });
});
```

This is commonly called **callback hell** or the **pyramid of doom**.

```text
getUser(...)
    └── getOrders(...)
          └── getPayment(...)
                └── sendEmail(...)
```

It becomes difficult to:

- Read
- Understand
- Maintain
- Debug
- Handle errors

Promises and `async/await` provide cleaner ways to structure dependent asynchronous operations.

---

## 7. What is Inversion of Control in callbacks?

When we pass a callback to another function, we give that function control over **when and how the callback is called**.

Example:

```js
function processData(callback) {
    // Some work
    callback();
}

processData(() => {
    console.log("Done");
});
```

We gave `processData()` our callback.

Now `processData()` decides when to execute it.

This is called **Inversion of Control**.

Normally, we control our own function:

```js
function doSomething() {
    console.log("Done");
}

doSomething();
```

With a callback:

```js
someFunction(doSomething);
```

we hand `doSomething` to another function and trust that function to call it correctly.

### Why can this be a problem?

Suppose we expect the callback to run exactly once:

```js
doWork(() => {
    console.log("Finished");
});
```

What if `doWork()`:

- Never calls the callback?
- Calls it twice?
- Calls it too early?
- Calls it with incorrect data?
- Calls it after an unexpected error?

The caller has less control.

Promises help by representing the eventual result of an asynchronous operation in a standard way.

---

# 8. Quick Revision

### How does JavaScript execute code?

JavaScript executes JavaScript code using a **Call Stack**, generally one operation at a time.

### Sync vs Async

**Synchronous:** wait for the current operation to finish.

**Asynchronous:** start an operation and allow other JavaScript work to continue while waiting.

### Ways to make code asynchronous

```text
Callbacks
Timers
Promises
Async/Await
```

### Web Browser APIs

Features provided by the browser environment, such as:

```text
Timers
DOM
Fetch/networking
Events
Storage
```

### Event Loop

The Event Loop coordinates when asynchronous callbacks can return for JavaScript execution after the Call Stack is ready.

### Callback Hell

Too many nested callbacks make asynchronous code difficult to read and maintain.

### Inversion of Control

Passing a callback to another function means that the receiving function controls when and how that callback is executed.

---

# 9. Practice Examples

## Example 1: Predict the output

```js
console.log("A");

setTimeout(() => {
    console.log("B");
}, 0);

console.log("C");
```

Answer:

```text
A
C
B
```

Reason: the timer callback runs after the current synchronous code finishes.


# Final Mental Model

```text
                JavaScript Code
                      │
                      ↓
                 Call Stack
                      │
              ┌───────┴────────┐
              │                │
       Synchronous       Async operation
       code executes     handled by environment
                              │
                              ↓
                         Callback ready
                              │
                              ↓
                         Event Loop
                              │
                              ↓
                         Call Stack
                              │
                              ↓
                      Callback executes
```

The main idea:

> **JavaScript executes one piece of JavaScript at a time, while the surrounding environment can handle asynchronous work and the Event Loop helps bring completed callbacks back for execution.**

Once this model is clear, learning **Promises** becomes much easier.
