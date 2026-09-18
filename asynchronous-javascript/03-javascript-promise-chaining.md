# JavaScript Promise Chaining

## 1. What is Promise Chaining?

**Promise chaining** means connecting multiple `.then()` calls so that the result of one asynchronous operation can be passed to the next operation.

Example:

```js
Promise.resolve(10)
    .then((value) => {
        return value * 2;
    })
    .then((value) => {
        return value + 5;
    })
    .then((value) => {
        console.log(value);
    });
```

Output:

```text
25
```

The flow is:

```text
10
 ↓
× 2
 ↓
20
 ↓
+ 5
 ↓
25
```

The important rule is:

> **A `.then()` returns a new Promise.**

That is why another `.then()` can be attached after it.

---

# 2. How do we chain Promises using `.then()`?

Suppose we have three operations:

```text
Get user
   ↓
Get orders
   ↓
Get payment
```

We can write:

```js
getUser()
    .then((user) => {
        return getOrders(user);
    })
    .then((orders) => {
        return getPayment(orders);
    })
    .then((payment) => {
        console.log(payment);
    });
```

### Why do we use `return`?

The returned value becomes the input to the next `.then()`.

```js
Promise.resolve(10)
    .then((value) => {
        return value * 2;
    })
    .then((value) => {
        console.log(value);
    });
```

Output:

```text
20
```

Think of it as:

```text
First .then()
     │
     │ returns 20
     ↓
Second .then()
     │
     │ receives 20
     ↓
console.log(20)
```

### What if we forget `return`?

```js
Promise.resolve(10)
    .then((value) => {
        value * 2;
    })
    .then((value) => {
        console.log(value);
    });
```

Output:

```text
undefined
```

The first `.then()` did not return the result.

So:

```js
return value * 2;
```

is important when the next step needs that value.

---

# 3. What happens when a `.then()` returns a Promise?

A `.then()` can return:

- A normal value
- Another Promise

Example:

```js
Promise.resolve(10)
    .then((value) => {
        return new Promise((resolve) => {
            setTimeout(() => {
                resolve(value * 2);
            }, 1000);
        });
    })
    .then((value) => {
        console.log(value);
    });
```

After one second:

```text
20
```

The second `.then()` waits for the Promise returned by the first `.then()`.

Mental model:

```text
.then()
  │
  ├── returns normal value
  │       ↓
  │   next .then()
  │
  └── returns Promise
          ↓
     wait for Promise
          ↓
      next .then()
```

This is one of the most important Promise-chaining rules.

---

# 4. How do we handle errors in a Promise chain using `.catch()`?

Use `.catch()` to handle a rejected Promise or an error thrown inside the chain.

Example:

```js
Promise.reject(new Error("Something failed"))
    .then((value) => {
        console.log(value);
    })
    .catch((error) => {
        console.log(error.message);
    });
```

Output:

```text
Something failed
```

A common pattern is:

```js
someOperation()
    .then((result) => {
        return anotherOperation(result);
    })
    .then((result) => {
        return anotherOperation(result);
    })
    .catch((error) => {
        console.error(error);
    });
```

The `.catch()` can handle failures from earlier parts of the chain.

---

# 5. What is `.finally()` in a Promise chain?

`.finally()` is used for code that should run after the Promise settles.

A Promise **settles** when it becomes either:

```text
fulfilled
OR
rejected
```

Example:

```js
Promise.resolve("Success")
    .then((value) => {
        console.log(value);
    })
    .finally(() => {
        console.log("Finished");
    });
```

Output:

```text
Success
Finished
```

It also runs when the Promise rejects:

```js
Promise.reject(new Error("Failed"))
    .catch((error) => {
        console.log(error.message);
    })
    .finally(() => {
        console.log("Finished");
    });
```

Output:

```text
Failed
Finished
```

### When is `finally()` useful?

It is useful for cleanup:

```text
Start loading
     ↓
Request finishes
     ↓
Stop loading
```

Example:

```js
showLoading();

fetchData()
    .then((data) => {
        console.log(data);
    })
    .catch((error) => {
        console.error(error);
    })
    .finally(() => {
        hideLoading();
    });
```

Whether the request succeeds or fails, `hideLoading()` runs.

---

# 6. What happens when an Error is thrown inside `.then()` and there is a `.catch()`?

If an error is thrown inside a `.then()` callback, the Promise returned by that `.then()` becomes rejected.

Example:

```js
Promise.resolve("Start")
    .then(() => {
        throw new Error("Something went wrong");
    })
    .catch((error) => {
        console.log(error.message);
    });
```

Output:

```text
Something went wrong
```

Flow:

```text
Promise fulfilled
       ↓
.then()
       ↓
throw Error
       ↓
Promise becomes rejected
       ↓
.catch()
       ↓
Error handled
```

This is why `.catch()` is able to handle errors thrown inside earlier `.then()` callbacks.

---

# 7. What happens when an Error is thrown inside `.then()` and there is NO `.catch()`?

Example:

```js
Promise.resolve("Start")
    .then(() => {
        throw new Error("Something went wrong");
    });
```

There is no `.catch()`.

The Promise becomes rejected, but there is no handler in this chain to handle that rejection.

This can result in an **unhandled Promise rejection** being reported by the runtime.

In Node.js or a browser, the exact console output/behavior can depend on the runtime and environment.

Important idea:

```text
throw Error
    ↓
Promise becomes rejected
    ↓
No .catch()
    ↓
Unhandled rejection
```

So asynchronous code should have an appropriate error-handling path.

---

# 8. Why should `.catch()` generally be placed toward the end of the Promise chain?

Consider:

```js
firstOperation()
    .then((result) => secondOperation(result))
    .then((result) => thirdOperation(result))
    .catch((error) => {
        console.error(error);
    });
```

A `.catch()` near the end can handle failures from the earlier chain.

For example:

```text
firstOperation()
      ↓
    .then()
      ↓
secondOperation()
      ↓
    .then()
      ↓
thirdOperation()
      ↓
    .catch()
```

If an earlier operation rejects or a `.then()` throws an error, the rejection travels down the chain until a rejection handler handles it.

### Important nuance

This does **not** mean `.catch()` must always be at the very end.

Sometimes we intentionally handle an error earlier and recover:

```js
getData()
    .catch((error) => {
        console.log("Using default data");
        return defaultData;
    })
    .then((data) => {
        console.log(data);
    });
```

Here the `.catch()` is deliberately in the middle because we want to recover and continue.

So the better rule is:

> Put `.catch()` where you want to handle the error. A catch near the end is common when one handler should handle failures from the whole chain.

---

# 9. What happens after a `.catch()` handles an error?

A `.catch()` also returns a new Promise.

Therefore, the chain can continue.

Example:

```js
Promise.reject(new Error("Failed"))
    .catch((error) => {
        console.log(error.message);
        return "Default value";
    })
    .then((value) => {
        console.log(value);
    });
```

Output:

```text
Failed
Default value
```

The `.catch()` handled the rejection and returned a normal value.

The next `.then()` receives that value.

```text
Rejected Promise
       ↓
.catch()
       ↓
returns "Default value"
       ↓
next .then()
       ↓
"Default value"
```

This is called **recovery** from an error.

---

# 10. What happens if `.catch()` throws another error?

Example:

```js
Promise.reject(new Error("First error"))
    .catch((error) => {
        console.log(error.message);
        throw new Error("Second error");
    })
    .catch((error) => {
        console.log(error.message);
    });
```

Output:

```text
First error
Second error
```

The first `.catch()` handles the first error but then throws another error.

That creates a rejected Promise, which is handled by the next `.catch()`.

---

# 11. How do we consume multiple Promises by chaining?

Suppose:

```text
Get user
   ↓
Get user's orders
   ↓
Get payment information
```

Each operation depends on the previous result.

We can chain them:

```js
function getUser() {
    return Promise.resolve({ id: 101 });
}

function getOrders(user) {
    return Promise.resolve({
        userId: user.id,
        orderId: 5001
    });
}

function getPayment(order) {
    return Promise.resolve({
        orderId: order.orderId,
        status: "Paid"
    });
}

getUser()
    .then((user) => {
        return getOrders(user);
    })
    .then((order) => {
        return getPayment(order);
    })
    .then((payment) => {
        console.log(payment);
    })
    .catch((error) => {
        console.error(error);
    });
```

Output:

```text
{ orderId: 5001, status: 'Paid' }
```

The important point is that these operations are **dependent**.

```text
User
 ↓
Orders needs User
 ↓
Payment needs Order
```

Chaining is suitable here.

---

# 12. Chaining vs `Promise.all()`

There is an important difference.

### Chaining

Use chaining when the next operation depends on the previous result.

```text
A
↓
B
↓
C
```

Example:

```js
getUser()
    .then((user) => getOrders(user))
    .then((orders) => getPayment(orders));
```

### `Promise.all()`

Use `Promise.all()` when operations can run independently and you need all their results.

```text
A ─────┐
B ─────┼──→ all results
C ─────┘
```

Example:

```js
const userPromise = getUser();
const productsPromise = getProducts();
const settingsPromise = getSettings();

Promise.all([
    userPromise,
    productsPromise,
    settingsPromise
])
    .then(([user, products, settings]) => {
        console.log(user);
        console.log(products);
        console.log(settings);
    })
    .catch((error) => {
        console.error(error);
    });
```

The three operations can proceed independently.

---

# 13. Why not simply chain independent Promises?

Suppose:

```js
getUser()
    .then(() => getProducts())
    .then(() => getSettings());
```

This creates a dependency/order:

```text
getUser
   ↓
getProducts
   ↓
getSettings
```

If `getProducts()` does not need the user result, making it wait unnecessarily can reduce concurrency.

Instead:

```js
Promise.all([
    getUser(),
    getProducts(),
    getSettings()
])
.then(([user, products, settings]) => {
    // use all results
});
```

Mental model:

```text
Dependent operations:
A → B → C
Use chaining

Independent operations:
A ─┐
B ─┼→ Promise.all()
C ─┘
```

---

# 14. How do we handle errors in a Promise chain?

A common pattern:

```js
getUser()
    .then((user) => {
        return getOrders(user);
    })
    .then((orders) => {
        return getPayment(orders);
    })
    .then((payment) => {
        console.log(payment);
    })
    .catch((error) => {
        console.error("Operation failed:", error.message);
    })
    .finally(() => {
        console.log("Finished");
    });
```

The flow is:

```text
Success
   ↓
then
   ↓
then
   ↓
then
   ↓
finally

Failure at any stage
   ↓
catch
   ↓
finally
```

This gives the asynchronous operation a clear success path, failure path, and cleanup path.

---

# 15. What happens when an error occurs in the middle of a chain?

Example:

```js
Promise.resolve("A")
    .then((value) => {
        console.log(value);
        return "B";
    })
    .then(() => {
        throw new Error("Failed at step 2");
    })
    .then(() => {
        console.log("This will not run");
    })
    .catch((error) => {
        console.log(error.message);
    });
```

Output:

```text
A
Failed at step 2
```

The `.then()` after the error is skipped because the chain is rejected.

Then `.catch()` handles the rejection.

Flow:

```text
.then() → A
   ↓
.then() → Error
   ↓
.then() → skipped
   ↓
.catch() → handles error
```

---

# 16. Why is error handling one of the most important parts of using Promises?

Asynchronous operations can fail for many reasons:

```text
Network failure
File not found
Invalid input
Server error
Authentication failure
Database failure
Timeout
```

A successful path alone is not enough.

Bad:

```js
getData()
    .then((data) => {
        console.log(data);
    });
```

Better:

```js
getData()
    .then((data) => {
        console.log(data);
    })
    .catch((error) => {
        console.error("Failed:", error.message);
    });
```

Error handling helps us:

- Detect failures
- Understand what happened
- Recover when possible
- Show useful messages
- Prevent unhandled rejections
- Make debugging easier

Think of every asynchronous operation as having two possible paths:

```text
             Async operation
                  │
          ┌───────┴───────┐
          ↓               ↓
       Success          Failure
          ↓               ↓
       .then()         .catch()
```

---

# 17. What does `finally()` do when there is an error?

Example:

```js
Promise.reject(new Error("Failed"))
    .catch((error) => {
        console.log(error.message);
    })
    .finally(() => {
        console.log("Cleanup complete");
    });
```

Output:

```text
Failed
Cleanup complete
```

`finally()` is useful for cleanup that should happen regardless of success or failure.

Examples:

```text
Hide loading spinner
Close a resource
Reset UI state
Stop a timer
Release temporary state
```

`finally()` normally does not receive the fulfillment value or rejection reason.

---

# 18. What happens if `finally()` itself throws an error?

Example:

```js
Promise.resolve("Success")
    .finally(() => {
        throw new Error("Cleanup failed");
    })
    .catch((error) => {
        console.log(error.message);
    });
```

Output:

```text
Cleanup failed
```

An error thrown inside `finally()` causes the resulting Promise to be rejected.

So `finally()` can also affect the final outcome of a chain.

---

# 19. Important Promise-chain rule: each method returns a new Promise

Consider:

```js
const promise1 = Promise.resolve(10);

const promise2 = promise1.then((value) => {
    return value * 2;
});

const promise3 = promise2.then((value) => {
    return value + 5;
});
```

Conceptually:

```text
promise1 → 10
    ↓
promise2 → 20
    ↓
promise3 → 25
```

This is why chaining works.

We can write it more compactly:

```js
Promise.resolve(10)
    .then((value) => value * 2)
    .then((value) => value + 5)
    .then((value) => console.log(value));
```

Output:

```text
25
```

---

# 20. Promise chaining vs callback nesting

### Callback nesting

```js
getUser((user) => {
    getOrders(user, (orders) => {
        getPayment(orders, (payment) => {
            console.log(payment);
        });
    });
});
```

### Promise chaining

```js
getUser()
    .then((user) => getOrders(user))
    .then((orders) => getPayment(orders))
    .then((payment) => {
        console.log(payment);
    })
    .catch((error) => {
        console.error(error);
    });
```

The Promise version gives us a flatter structure and a clearer error-handling path.

---

# 21. A complete practical example

Imagine an application that needs to:

1. Get a user
2. Get the user's orders
3. Calculate the total
4. Display the total

```js
function getUser() {
    return Promise.resolve({
        id: 101,
        name: "Adi"
    });
}

function getOrders(user) {
    return Promise.resolve([
        { price: 500 },
        { price: 300 },
        { price: 200 }
    ]);
}

function calculateTotal(orders) {
    return Promise.resolve(
        orders.reduce((total, order) => total + order.price, 0)
    );
}

getUser()
    .then((user) => {
        console.log("User:", user.name);
        return getOrders(user);
    })
    .then((orders) => {
        console.log("Orders:", orders.length);
        return calculateTotal(orders);
    })
    .then((total) => {
        console.log("Total:", total);
    })
    .catch((error) => {
        console.error("Error:", error.message);
    })
    .finally(() => {
        console.log("Process finished");
    });
```

Output:

```text
User: Adi
Orders: 3
Total: 1000
Process finished
```

Flow:

```text
getUser()
   ↓
user
   ↓
getOrders(user)
   ↓
orders
   ↓
calculateTotal(orders)
   ↓
total
   ↓
display total
   ↓
finally()
```

---

# 22. Quick Revision

### Promise chaining

```js
promise
    .then(...)
    .then(...)
    .then(...)
    .catch(...)
    .finally(...);
```

### `.then()`

Handles a fulfilled result and returns a new Promise.

### Return from `.then()`

```js
.then((value) => {
    return newValue;
})
```

`newValue` becomes the input of the next `.then()`.

### Return another Promise

```js
.then(() => {
    return anotherPromise();
})
```

The next `.then()` waits for it.

### `.catch()`

Handles rejection and errors thrown in earlier parts of the chain.

### `.finally()`

Runs after settlement and is commonly used for cleanup.

### Error propagation

```text
Error/rejection
      ↓
skip normal .then() handlers
      ↓
find a rejection handler
      ↓
.catch()
```

### Chaining vs `Promise.all()`

```text
Dependent:
A → B → C
Use chaining

Independent:
A ─┐
B ─┼→ Promise.all()
C ─┘
```


# Final Mental Model

The most important picture to remember:

```text
                Promise
                   │
                 .then()
                   │
              return value
                   │
                   ↓
                .then()
                   │
          return Promise
                   │
                   ↓
             wait for it
                   │
                   ↓
                .then()
                   │
             Error occurs?
                   │
                   ↓
                .catch()
                   │
             recover/handle
                   │
                   ↓
               .finally()
                   │
                   ↓
                 Done
```

For multiple operations:

```text
DEPENDENT OPERATIONS

A
↓
B
↓
C

Use Promise chaining.


INDEPENDENT OPERATIONS

A ─────┐
B ─────┼──→ Promise.all()
C ─────┘
```

The key rules are:

> **Return values to pass data forward.**

> **Return Promises when the next step must wait for asynchronous work.**

> **Use `.catch()` to handle failures.**

> **Use `.finally()` for cleanup.**

> **Use chaining for dependent operations and `Promise.all()` for independent operations.**
