# JavaScript Promises

## 1. What is a Promise?

A **Promise** is an object that represents the eventual result of an asynchronous operation.

It can be thought of as a promise about a value that will be available later.

For example, imagine asking a server for user data:

```text
Request sent
     ↓
Promise
     ↓
waiting...
     ↓
data available
```

A Promise does not contain the final result immediately. It represents the operation and its future result.

Example:

```js
const promise = new Promise((resolve, reject) => {
    resolve("Data received");
});

console.log(promise);
```

A Promise can eventually:

```text
success → fulfilled
failure → rejected
```

---

## 2. Why do we use Promises?

Before Promises, asynchronous operations were commonly handled using callbacks.

Example:

```js
getUser((user) => {
    getOrders(user, (orders) => {
        console.log(orders);
    });
});
```

When many operations depend on each other, callbacks can become deeply nested.

Promises give us a cleaner way to represent asynchronous results.

Example:

```js
getUser()
    .then((user) => getOrders(user))
    .then((orders) => {
        console.log(orders);
    })
    .catch((error) => {
        console.error(error);
    });
```

The important idea is:

> A Promise represents a result that may be available now, later, or may fail.

---

# 3. How do we create a new Promise?

We create a Promise using the `Promise` constructor.

Syntax:

```js
const promise = new Promise((resolve, reject) => {
    // asynchronous work

    if (/* successful */) {
        resolve(value);
    } else {
        reject(error);
    }
});
```

The function passed to `new Promise()` is called the **executor function**.

It receives two functions:

```text
resolve → successful result
reject  → failed result
```

Example:

```js
const promise = new Promise((resolve, reject) => {
    const success = true;

    if (success) {
        resolve("Operation successful");
    } else {
        reject("Operation failed");
    }
});
```

Here:

```js
resolve("Operation successful");
```

fulfills the Promise.

And:

```js
reject("Operation failed");
```

rejects the Promise.

---

## 4. Simple asynchronous Promise example

```js
const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve("Data loaded");
    }, 2000);
});

console.log("Waiting...");

promise.then((result) => {
    console.log(result);
});
```

Output:

```text
Waiting...
Data loaded
```

The Promise is initially waiting while the timer is running.

After two seconds:

```text
resolve("Data loaded")
        ↓
Promise fulfilled
        ↓
.then() callback runs
```

---

# 5. What are the different states of a Promise?

A Promise has three main states:

```text
             Pending
             /                 /                  ↓         ↓
      Fulfilled    Rejected
```

## Pending

The operation has started but has not finished yet.

```js
const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve("Done");
    }, 2000);
});
```

For those two seconds, the Promise is pending.

---

## Fulfilled

The operation completed successfully.

```js
resolve("Success");
```

The Promise becomes fulfilled.

---

## Rejected

The operation failed.

```js
reject(new Error("Something went wrong"));
```

The Promise becomes rejected.

### Important

A Promise can move only once from the pending state.

```text
Pending
   ↓
Fulfilled
```

or

```text
Pending
   ↓
Rejected
```

It cannot go back to pending.

It also cannot change from fulfilled to rejected or rejected to fulfilled.

---

# 6. What is `resolve()`?

`resolve()` tells the Promise:

> The asynchronous operation completed successfully.

Example:

```js
const promise = new Promise((resolve, reject) => {
    resolve("Success");
});
```

The Promise becomes fulfilled with:

```text
"Success"
```

That value can later be received using `.then()`.

```js
promise.then((value) => {
    console.log(value);
});
```

Output:

```text
Success
```

---

# 7. What is `reject()`?

`reject()` tells the Promise:

> The asynchronous operation failed.

Example:

```js
const promise = new Promise((resolve, reject) => {
    reject(new Error("Network failed"));
});
```

We can handle the rejection using `.catch()`:

```js
promise.catch((error) => {
    console.log(error.message);
});
```

Output:

```text
Network failed
```

It is generally better to reject with an `Error` object:

```js
reject(new Error("Something went wrong"));
```

instead of:

```js
reject("Something went wrong");
```

because an `Error` provides useful information such as `message` and `stack`.

---

# 8. How do we consume an existing Promise?

**Consuming a Promise** means using the result of a Promise.

The most common methods are:

```text
.then()
.catch()
.finally()
```

Example:

```js
const promise = Promise.resolve("Hello");

promise.then((value) => {
    console.log(value);
});
```

Output:

```text
Hello
```

### `.then()`

Used when the Promise is fulfilled.

```js
promise.then((value) => {
    console.log(value);
});
```

### `.catch()`

Used when the Promise is rejected.

```js
promise.catch((error) => {
    console.error(error);
});
```

### `.finally()`

Runs after the Promise settles, whether fulfilled or rejected.

```js
promise.finally(() => {
    console.log("Finished");
});
```

More detailed Promise chaining is covered in the next technical paper.

---

# 9. How does a Promise return a value?

Example:

```js
const promise = new Promise((resolve) => {
    resolve(10);
});

promise.then((value) => {
    console.log(value);
});
```

Output:

```text
10
```

The value passed to `resolve()` becomes the value received by `.then()`.

```text
resolve(10)
    ↓
Promise fulfilled
    ↓
.then((value) => ...)
    ↓
value = 10
```

---

# 10. How does a Promise return another Promise?

A `.then()` callback can return another Promise.

Example:

```js
const firstPromise = Promise.resolve(10);

firstPromise
    .then((value) => {
        return Promise.resolve(value * 2);
    })
    .then((value) => {
        console.log(value);
    });
```

Output:

```text
20
```

The second `.then()` waits for the returned Promise.

This behavior is the foundation of **Promise chaining**.

---

# 11. What is `Promise.resolve()`?

`Promise.resolve()` creates a fulfilled Promise.

```js
const promise = Promise.resolve("Success");

promise.then((value) => {
    console.log(value);
});
```

Output:

```text
Success
```

It is useful when we already have a value but want to work with it as a Promise.

Example:

```js
Promise.resolve(100)
    .then((value) => {
        console.log(value);
    });
```

Output:

```text
100
```

If the value passed to `Promise.resolve()` is already a Promise, it returns/adopts that Promise rather than creating an unrelated fulfilled Promise around it.

---

# 12. What is `Promise.reject()`?

`Promise.reject()` creates a rejected Promise.

```js
const promise = Promise.reject(new Error("Failed"));

promise.catch((error) => {
    console.log(error.message);
});
```

Output:

```text
Failed
```

It is useful when we need a rejected Promise directly.

---

# 13. What is `Promise.all()`?

`Promise.all()` is used when we have multiple Promises and want to wait for **all of them to fulfill**.

Example:

```js
const promise1 = Promise.resolve("A");
const promise2 = Promise.resolve("B");
const promise3 = Promise.resolve("C");

Promise.all([promise1, promise2, promise3])
    .then((results) => {
        console.log(results);
    });
```

Output:

```text
[ 'A', 'B', 'C' ]
```

The result array follows the **order of the input Promises**, not necessarily the order in which they finish.

### If one Promise rejects

```js
const promise1 = Promise.resolve("A");
const promise2 = Promise.reject(new Error("Failed"));
const promise3 = Promise.resolve("C");

Promise.all([promise1, promise2, promise3])
    .then((results) => {
        console.log(results);
    })
    .catch((error) => {
        console.log(error.message);
    });
```

Output:

```text
Failed
```

`Promise.all()` rejects when one of its input Promises rejects.

### When to use it?

Use `Promise.all()` when all operations are required.

Example:

```text
Get user
Get orders
Get payments

       ↓

Need all three results
       ↓
Promise.all()
```

---

# 14. What is `Promise.allSettled()`?

`Promise.allSettled()` waits for **all Promises to finish**, whether they fulfill or reject.

Example:

```js
const promise1 = Promise.resolve("A");
const promise2 = Promise.reject(new Error("Failed"));
const promise3 = Promise.resolve("C");

Promise.allSettled([promise1, promise2, promise3])
    .then((results) => {
        console.log(results);
    });
```

The result contains the status of every Promise.

Conceptually:

```js
[
    { status: "fulfilled", value: "A" },
    { status: "rejected", reason: Error(...) },
    { status: "fulfilled", value: "C" }
]
```

Unlike `Promise.all()`, one rejection does not make `Promise.allSettled()` reject.

### When to use it?

Use it when you want to know the result of **every operation**, even if some fail.

Example:

```text
Send notification to 5 users
       ↓
Some succeed
Some fail
       ↓
Need results for all 5
       ↓
Promise.allSettled()
```

---

# 15. What is `Promise.any()`?

`Promise.any()` waits for the **first fulfilled Promise**.

Example:

```js
const promise1 = Promise.reject("Failed");
const promise2 = Promise.resolve("Success");
const promise3 = Promise.resolve("Another success");

Promise.any([promise1, promise2, promise3])
    .then((result) => {
        console.log(result);
    });
```

Output:

```text
Success
```

It ignores rejected Promises until it finds a fulfilled one.

### What if all Promises reject?

Then `Promise.any()` rejects with an `AggregateError`.

Conceptually:

```text
Promise 1 → rejected
Promise 2 → rejected
Promise 3 → rejected

        ↓

No successful Promise

        ↓

AggregateError
```

### When to use it?

Useful when several alternatives can provide the result and **any one successful result is enough**.

---

# 16. What is `Promise.race()`?

`Promise.race()` settles as soon as the **first input Promise settles**.

"Settles" means:

```text
fulfilled OR rejected
```

Example:

```js
const promise1 = new Promise((resolve) => {
    setTimeout(() => resolve("Slow"), 2000);
});

const promise2 = new Promise((resolve) => {
    setTimeout(() => resolve("Fast"), 500);
});

Promise.race([promise1, promise2])
    .then((result) => {
        console.log(result);
    });
```

Output:

```text
Fast
```

The second Promise settles first.

### Important difference

```text
Promise.any()
    → first FULFILLED Promise

Promise.race()
    → first SETTLED Promise
       (fulfilled or rejected)
```

---

# 17. Comparison of Promise methods

| Method | Waits for | If one rejects |
|---|---|---|
| `Promise.all()` | All to fulfill | Rejects |
| `Promise.allSettled()` | All to settle | Does not reject because of an input rejection |
| `Promise.any()` | First fulfillment | Rejects only if all reject |
| `Promise.race()` | First settlement | Follows the first settled result |

Mental model:

```text
Promise.all()
→ "I need EVERY successful result."

Promise.allSettled()
→ "Tell me what happened to EVERY operation."

Promise.any()
→ "Give me the FIRST successful result."

Promise.race()
→ "Give me whatever settles FIRST."
```

---

# 18. How do we promisify an asynchronous callback-based function?

**Promisification** means converting a callback-based asynchronous function into a function that returns a Promise.

For example, suppose we have:

```js
function wait(callback) {
    setTimeout(() => {
        callback("Finished");
    }, 1000);
}
```

We can convert it into a Promise-based function:

```js
function waitPromise() {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve("Finished");
        }, 1000);
    });
}
```

Now we can use:

```js
waitPromise()
    .then((result) => {
        console.log(result);
    });
```

Output after one second:

```text
Finished
```

---

# 19. Promisifying `setTimeout`

A reusable version:

```js
function delay(milliseconds) {
    return new Promise((resolve) => {
        setTimeout(resolve, milliseconds);
    });
}
```

Usage:

```js
console.log("Start");

delay(2000)
    .then(() => {
        console.log("Two seconds finished");
    });
```

Output:

```text
Start
Two seconds finished
```

This is useful because `setTimeout()` itself does not return a Promise.

---

# 20. Promisifying `fs.readFile`

Node.js provides callback-based APIs as well as Promise-based APIs.

A traditional callback version can look like:

```js
const fs = require("fs");

fs.readFile("data.txt", "utf8", (error, data) => {
    if (error) {
        console.error(error);
        return;
    }

    console.log(data);
});
```

We can manually wrap it in a Promise:

```js
const fs = require("fs");

function readFilePromise(filePath) {
    return new Promise((resolve, reject) => {
        fs.readFile(filePath, "utf8", (error, data) => {
            if (error) {
                reject(error);
                return;
            }

            resolve(data);
        });
    });
}
```

Now:

```js
readFilePromise("data.txt")
    .then((data) => {
        console.log(data);
    })
    .catch((error) => {
        console.error(error.message);
    });
```

The pattern is:

```text
Callback API
     ↓
check error
     ↓
error → reject()
success → resolve()
     ↓
Promise
```

Node.js also provides built-in Promise-based APIs, so manual promisification is not always necessary.

---

# 21. How should we handle errors with Promises?

Use `.catch()` for rejected Promises.

```js
const promise = Promise.reject(new Error("Something went wrong"));

promise
    .then((value) => {
        console.log(value);
    })
    .catch((error) => {
        console.error(error.message);
    });
```

Output:

```text
Something went wrong
```

A good pattern is:

```js
someAsyncOperation()
    .then((result) => {
        // success
    })
    .catch((error) => {
        // failure
    });
```

We will study detailed Promise-chain error propagation in the next paper.

---

# 22. Why is error handling important when using Promises?

Asynchronous operations can fail.

For example:

```text
Network request
File reading
Database operation
API request
Authentication
```

If a Promise rejects and nobody handles the rejection, the application can end up with an **unhandled Promise rejection**.

Therefore, asynchronous code should have a clear failure path.

```text
Async operation
      │
      ├── Success → resolve() → .then()
      │
      └── Failure → reject() → .catch()
```

Good error handling helps us:

- Understand what failed
- Show useful messages
- Prevent unexpected application behavior
- Debug problems
- Recover when possible

---

# 23. Important Promise rules

### Rule 1: A Promise settles only once

```js
const promise = new Promise((resolve, reject) => {
    resolve("First");

    resolve("Second");
    reject("Error");
});
```

Only the first settlement matters.

The Promise becomes fulfilled with:

```text
First
```

Later calls do not change the settled Promise.

---

### Rule 2: `resolve()` does not mean the `.then()` callback runs immediately

```js
const promise = new Promise((resolve) => {
    resolve("Done");
});

console.log("A");

promise.then((value) => {
    console.log(value);
});

console.log("B");
```

Output:

```text
A
B
Done
```

Even an already fulfilled Promise runs its `.then()` callback asynchronously through the Promise job/microtask mechanism.

---

### Rule 3: Returning a Promise from `.then()` matters

```js
Promise.resolve(10)
    .then((value) => {
        return Promise.resolve(value * 2);
    })
    .then((value) => {
        console.log(value);
    });
```

Output:

```text
20
```

The next `.then()` waits for the returned Promise.

---

# 24. Quick Revision

```text
Promise
→ represents an eventual result.

new Promise()
→ creates a Promise.

resolve()
→ success.

reject()
→ failure.

Pending
→ still waiting.

Fulfilled
→ completed successfully.

Rejected
→ completed with failure.

.then()
→ consume a fulfilled result.

.catch()
→ handle rejection.

.finally()
→ run cleanup after settlement.

Promise.resolve()
→ create/adopt a fulfilled Promise.

Promise.reject()
→ create a rejected Promise.

Promise.all()
→ all must fulfill.

Promise.allSettled()
→ wait for all results.

Promise.any()
→ first fulfilled result.

Promise.race()
→ first settled result.

Promisification
→ convert callback-based async code into Promise-based code.
```


# Final Mental Model

Keep this picture in mind:

```text
                  Promise
                     │
                 ┌───┴───┐
                 ↓       ↓
              Pending   ...
                 │
          ┌──────┴──────┐
          ↓             ↓
      Fulfilled      Rejected
          │             │
        .then()       .catch()
          │             │
          └──────┬──────┘
                 ↓
              .finally()
```

And for multiple Promises:

```text
Promise.all()
→ ALL successful

Promise.allSettled()
→ ALL finished

Promise.any()
→ FIRST successful

Promise.race()
→ FIRST finished
```

Once these Promise basics are clear, the next important topic is **Promise Chaining**: how values and errors move from one `.then()` to the next, how `.catch()` handles errors, and how multiple asynchronous operations can be combined.
