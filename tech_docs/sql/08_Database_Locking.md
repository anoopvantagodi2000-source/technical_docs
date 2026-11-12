# Database Locking Mechanism

## 1. Introduction

A database is often used by many users and application processes at the
same time. Multiple transactions may try to read or modify the same
data.

A **lock** is a concurrency-control mechanism that helps coordinate
access to database resources and prevent conflicting operations.

## 2. Why Is Locking Needed?

Imagine two users trying to update the same bank account balance.

The balance is ₹1,000.

-   Transaction A wants to withdraw ₹200.
-   Transaction B wants to withdraw ₹300.

If both transactions read the original balance and then independently
write their calculated results, one update could overwrite the other.
This is an example of a lost update.

Database concurrency control helps prevent such conflicts.

## 3. Shared Lock

A shared lock is used when a transaction reads data under a locking
model.

Multiple transactions can generally hold compatible shared locks on the
same resource at the same time.

A conflicting write lock is not allowed until the shared locks are
released.

**Simple idea:** Multiple readers can access the data, but a conflicting
writer must wait.

## 4. Exclusive Lock

An exclusive lock is used when a transaction needs to modify data.

Other transactions cannot acquire conflicting locks on the same resource
while the exclusive lock is held.

**Simple idea:** A transaction that holds an exclusive lock has
protected access to modify the locked resource.

The exact lock behavior depends on the database engine and isolation
level.

## 5. Example in SQL

In PostgreSQL, a transaction can explicitly lock selected rows for
updating:

``` sql
BEGIN;

SELECT balance
FROM accounts
WHERE account_id = 1
FOR UPDATE;

UPDATE accounts
SET balance = balance - 200
WHERE account_id = 1;

COMMIT;
```

`FOR UPDATE` obtains row-level locks on the selected rows. Other
transactions attempting conflicting updates or row locks must wait until
the transaction finishes.

## 6. What Is a Deadlock?

A deadlock occurs when two or more transactions wait for resources held
by one another.

Example:

-   Transaction A locks Row 1 and waits for Row 2.
-   Transaction B locks Row 2 and waits for Row 1.

Neither transaction can continue until the deadlock is resolved.

Databases can detect deadlocks and typically abort one transaction so
the other can proceed.

## 7. How Can Deadlocks Be Reduced?

Developers can reduce the chance of deadlocks by:

1.  Accessing records in a consistent order.
2.  Keeping transactions short.
3.  Avoiding unnecessary locks.
4.  Handling deadlock errors and retrying transactions when appropriate.

## 8. Conclusion

Locking helps databases coordinate concurrent access to shared data. It
protects against conflicting operations, but excessive locking can
reduce performance and cause waiting.



## 10. SQL Example: Locking a Row

In PostgreSQL, `SELECT ... FOR UPDATE` locks selected rows so that another transaction cannot perform a conflicting update or acquire a conflicting row lock until the current transaction finishes.

Run this in Transaction A:

```sql
BEGIN;

SELECT balance
FROM accounts
WHERE account_id = 1
FOR UPDATE;

-- Keep this transaction open briefly.
-- Then finish it with COMMIT or ROLLBACK.
```

While Transaction A is still open, try this in Transaction B:

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 100
WHERE account_id = 1;

-- This update must wait for Transaction A's conflicting row lock.
```

After Transaction A commits or rolls back, Transaction B can continue. Finish Transaction B with:

```sql
COMMIT;
```

For a useful experiment, open two database sessions and run the statements in the order described. Do not leave transactions open longer than necessary.
