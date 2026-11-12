# Database Transactions

## 1. Introduction

A transaction is a group of database operations treated as one logical
unit of work.

A transaction is useful when multiple operations must succeed together.
If an operation fails before the transaction commits, the transaction
can be rolled back.

## 2. Example: Transferring Money

Imagine transferring ₹500 from Account A to Account B.

The transfer requires two updates:

1.  Deduct ₹500 from Account A.
2.  Add ₹500 to Account B.

If only the first update succeeds, the account balances become
incorrect. Both operations should therefore be part of one transaction.

## 3. Basic Transaction Commands

Common SQL transaction commands include:

-   `BEGIN` or `START TRANSACTION`: Starts a transaction.
-   `COMMIT`: Saves the transaction's changes.
-   `ROLLBACK`: Cancels uncommitted changes.

Example:

``` sql
BEGIN;

UPDATE accounts
SET balance = balance - 500
WHERE account_id = 1;

UPDATE accounts
SET balance = balance + 500
WHERE account_id = 2;

COMMIT;
```

If an error occurs and the transaction has not committed, the
application can issue:

``` sql
ROLLBACK;
```

This undoes the transaction's uncommitted changes.

## 4. Why Are Transactions Important?

Transactions help:

1.  Keep related operations together.
2.  Avoid partially completed business operations.
3.  Maintain data integrity.
4.  Coordinate concurrent work.
5.  Support reliable operations such as payments and order creation.

## 5. Transactions and ACID

Transactions are closely connected to ACID properties:

-   Atomicity: all operations succeed or none do.
-   Consistency: database rules remain satisfied.
-   Isolation: concurrent transactions are controlled.
-   Durability: committed changes remain saved.

## 6. Important Practical Considerations

A transaction should generally be kept as short as practical.
Long-running transactions can hold locks, delay other transactions, and
increase the chance of contention.

Also, a `COMMIT` should happen only after the application has completed
the operations that belong to the transaction.

## 7. Conclusion

Transactions allow applications to perform multiple database operations
safely as one unit. They are essential for business operations where
partial updates would cause incorrect data.



## 10. SQL Practice: Commit and Roll Back

Create a small accounts table:

```sql
CREATE TABLE accounts (
    account_id INT PRIMARY KEY,
    balance DECIMAL(10, 2) NOT NULL
);

INSERT INTO accounts (account_id, balance) VALUES
(1, 5000),
(2, 3000);
```

Transfer ₹500 and commit the transaction:

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 500
WHERE account_id = 1;

UPDATE accounts
SET balance = balance + 500
WHERE account_id = 2;

COMMIT;
```

Check the balances:

```sql
SELECT *
FROM accounts
ORDER BY account_id;
```

Now try a transaction and cancel it:

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 100
WHERE account_id = 1;

ROLLBACK;
```

The second update is undone because the transaction was rolled back before it was committed.
