# SQL Technical Paper

## 1. ACID

ACID properties make database transactions reliable and consistent.

### Atomicity

A transaction is treated as one unit. Either all operations are completed or none of them are completed.

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 500
WHERE id = 1;

UPDATE accounts
SET balance = balance + 500
WHERE id = 2;

COMMIT;
```

If an operation fails, `ROLLBACK` can be used so that the transaction does not leave partial changes.

### Consistency

A transaction should move the database from one valid state to another valid state. Database rules and constraints should not be violated.

### Isolation

When multiple transactions run at the same time, one transaction should not incorrectly interfere with another transaction.

### Durability

After a transaction is committed, the changes should remain saved even if the database system restarts.

---

## 2. CAP Theorem

CAP theorem is mainly used for distributed databases and distributed systems.

It describes three properties:

- **Consistency** - every node sees the same data.
- **Availability** - every request receives a response.
- **Partition Tolerance** - the system continues working even when communication between nodes is interrupted.

When a network partition occurs, a distributed system has to make a trade-off between consistency and availability.

CAP is about distributed systems, not about normal SQL queries.

---

## 3. Joins

A join combines rows from two or more tables using a related column.

For example:

```text
customers
id | name

orders
id | customer_id | amount
```

We can combine them using:

```sql
SELECT customers.name, orders.amount
FROM customers
JOIN orders
ON customers.id = orders.customer_id;
```

### INNER JOIN

Returns only rows that have a match in both tables.

```sql
SELECT c.name, o.amount
FROM customers c
INNER JOIN orders o
ON c.id = o.customer_id;
```

### LEFT JOIN

Returns all rows from the left table and matching rows from the right table. If there is no match, the right-side columns contain `NULL`.

```sql
SELECT c.name, o.amount
FROM customers c
LEFT JOIN orders o
ON c.id = o.customer_id;
```

### RIGHT JOIN

Returns all rows from the right table and matching rows from the left table.

### FULL OUTER JOIN

Returns matching rows plus unmatched rows from both tables.

### CROSS JOIN

Returns every possible combination of rows from the two tables.

### SELF JOIN

A table can be joined with itself. For example, an employee table can be joined with itself to find an employee's manager.

---

## 4. Aggregations and Filters

Aggregate functions perform calculations on multiple rows.

Common aggregate functions:

- `COUNT()` - counts rows
- `SUM()` - calculates a total
- `AVG()` - calculates an average
- `MIN()` - finds the minimum
- `MAX()` - finds the maximum

Example:

```sql
SELECT team, SUM(runs) AS total_runs
FROM matches
GROUP BY team;
```

### WHERE

`WHERE` filters individual rows before grouping.

```sql
SELECT *
FROM employees
WHERE salary > 50000;
```

### GROUP BY

`GROUP BY` creates groups so aggregate functions can be applied to each group.

```sql
SELECT department, COUNT(*)
FROM employees
GROUP BY department;
```

### HAVING

`HAVING` filters groups after `GROUP BY`.

```sql
SELECT department, COUNT(*)
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;
```

Easy way to remember:

```text
WHERE     -> filters rows
GROUP BY  -> creates groups
HAVING    -> filters groups
```

### ORDER BY

Sorts the result.

```sql
SELECT name, salary
FROM employees
ORDER BY salary DESC;
```

### LIMIT

Restricts the number of rows returned.

```sql
SELECT *
FROM employees
LIMIT 10;
```

---

## 5. Normalization

Normalization is the process of organizing data to reduce unnecessary duplication and improve consistency.

Instead of storing a customer's name repeatedly:

```text
orders
order_id | customer_name | amount
```

we can separate the data:

```text
customers
customer_id | customer_name

orders
order_id | customer_id | amount
```

The `customer_id` connects the two tables.

### 1NF

- Each column should contain atomic values.
- Avoid storing multiple values in one column.

### 2NF

- The table should already be in 1NF.
- Non-key columns should depend on the whole primary key.

### 3NF

- The table should already be in 2NF.
- Non-key columns should depend only on the key, not on another non-key column.

The main goal is to reduce duplication and avoid update, insert, and delete problems.

---

## 6. Indexes

An index helps the database find rows faster.

Without an index, the database may need to scan many rows to find matching data.

Example:

```sql
CREATE INDEX idx_employee_name
ON employees(name);
```

A query such as this can benefit from an index on `name`:

```sql
SELECT *
FROM employees
WHERE name = 'Ravi';
```

Indexes are commonly useful for columns used in:

- `WHERE`
- `JOIN`
- `ORDER BY`

Indexes also have a cost:

- They use additional storage.
- Inserts and updates can become slower because the index also needs to be updated.

Therefore, we should not create indexes on every column.

---

## 7. Transactions

A transaction is a group of database operations treated as one unit of work.

Important commands are:

```sql
BEGIN;
```

Starts a transaction.

```sql
COMMIT;
```

Permanently saves the changes.

```sql
ROLLBACK;
```

Cancels the changes made in the current transaction.

Example:

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 1000
WHERE id = 1;

UPDATE accounts
SET balance = balance + 1000
WHERE id = 2;

COMMIT;
```

If something goes wrong before the commit:

```sql
ROLLBACK;
```

can be used.

Transactions are especially important when several operations must succeed together.

---

## 8. Locking Mechanism

A lock controls access to data when multiple transactions work with the same database.

Its purpose is to prevent conflicting operations from causing incorrect results.

For example, a row can be locked explicitly using:

```sql
SELECT *
FROM accounts
WHERE id = 1
FOR UPDATE;
```

This is useful when an application needs to read a row and then safely update it within the same transaction.

PostgreSQL can also take locks automatically during operations such as `UPDATE` and `DELETE`.

Too many locks can make transactions wait for each other. Poorly managed transactions can also lead to deadlocks.

---

## 9. Database Isolation Levels

Isolation levels define how much one transaction can see from other transactions.

PostgreSQL supports:

- Read Uncommitted
- Read Committed
- Repeatable Read
- Serializable

In PostgreSQL, `Read Uncommitted` behaves like `Read Committed`.

### Read Committed

This is PostgreSQL's default isolation level. A query normally sees only data committed before that query began.

### Repeatable Read

The transaction works with a consistent snapshot. Repeated reads within the transaction see the same snapshot.

### Serializable

Provides the strongest isolation. Transactions behave as if they were executed one after another.

It gives stronger consistency but may cause a transaction to fail and require a retry when concurrent operations conflict.

### Common problems

**Dirty read:** Reading data that another transaction has not committed.

**Non-repeatable read:** Reading the same row twice and getting different committed values.

**Phantom read:** Repeating a query and finding new or removed rows that match the condition.

Simple idea:

```text
More isolation
      ↓
More consistency
      ↓
Usually more coordination / possible performance cost
```

---

## 10. Triggers

A trigger is a database action that automatically runs when a specified event happens on a table.

Common events are:

- `INSERT`
- `UPDATE`
- `DELETE`

Triggers can be used for:

- Keeping an audit table.
- Recording changes automatically.
- Validating or transforming data.
- Updating related information.

In PostgreSQL, a trigger normally calls a trigger function.

Example:

```sql
CREATE OR REPLACE FUNCTION log_employee_change()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO employee_log(employee_id, action)
    VALUES (NEW.id, 'INSERT');

    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

Then create the trigger:

```sql
CREATE TRIGGER employee_insert_log
AFTER INSERT ON employees
FOR EACH ROW
EXECUTE FUNCTION log_employee_change();
```

Now the trigger function runs automatically whenever a new employee is inserted.

`NEW` represents the new row in an `INSERT` or `UPDATE` trigger.

`OLD` represents the previous row in an `UPDATE` or `DELETE` trigger.

Triggers are useful, but they should be used carefully because automatic actions can make database behavior harder to understand and debug.

---

## 11. How the Main Concepts Connect

For writing queries:

```text
JOIN
  ↓
Combine data from tables

WHERE
  ↓
Filter rows

GROUP BY
  ↓
Create groups

Aggregate functions
  ↓
COUNT / SUM / AVG / MIN / MAX

HAVING
  ↓
Filter groups

ORDER BY
  ↓
Sort results

LIMIT
  ↓
Restrict the result
```

For database design and reliability:

```text
Normalization
  ↓
Reduce unnecessary duplication

Indexes
  ↓
Improve common searches

Transactions
  ↓
Group related operations safely

Locks + Isolation Levels
  ↓
Control concurrent transactions

ACID
  ↓
Make transactions reliable

Triggers
  ↓
Run database actions automatically

CAP
  ↓
Understand trade-offs in distributed systems
```
