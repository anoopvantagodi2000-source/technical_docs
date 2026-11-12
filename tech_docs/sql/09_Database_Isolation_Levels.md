# Database Isolation Levels

## 1. Introduction

Multiple transactions can run at the same time in a database. Isolation
levels define how much one transaction is protected from changes made by
other concurrent transactions.

Isolation levels help balance data correctness, concurrency, and
performance.

The four commonly discussed SQL isolation levels are:

1.  Read Uncommitted
2.  Read Committed
3.  Repeatable Read
4.  Serializable

Database implementations can differ, so the exact behavior should be
checked for the database being used.

## 2. Read Uncommitted

Read Uncommitted is the lowest standard isolation level.

A transaction may be allowed to read changes made by another transaction
before those changes are committed. This can cause a **dirty read**.

Example:

1.  Transaction A changes an account balance from ₹1,000 to ₹500 but
    does not commit.
2.  Transaction B reads the balance and sees ₹500.
3.  Transaction A rolls back.
4.  The actual balance returns to ₹1,000.

Transaction B read a value that was never committed.

Note: PostgreSQL treats `READ UNCOMMITTED` as `READ COMMITTED`, so it
does not allow dirty reads.

## 3. Read Committed

Read Committed allows a transaction to read only committed data.

It prevents dirty reads.

However, if a transaction runs the same query twice, another transaction
may commit an update between those reads. The first transaction may then
see a different value on the second read. This is called a
**non-repeatable read**.

PostgreSQL uses Read Committed as its default isolation level.

## 4. Repeatable Read

Repeatable Read ensures that data read by a transaction remains
consistent when it is read again within that transaction.

It prevents dirty reads and non-repeatable reads.

The handling of phantom reads can differ between database
implementations. PostgreSQL's Repeatable Read uses a transaction-level
snapshot and also prevents phantom reads in its implementation.

## 5. Serializable

Serializable is the strongest standard isolation level.

It aims to make concurrent transactions behave as if they had executed
one after another in some serial order.

This provides strong protection against concurrency anomalies, but
transactions may be aborted and need to be retried.

## 6. Common Concurrency Problems

### Dirty Read

A transaction reads data written by another transaction that has not
committed.

### Non-Repeatable Read

A transaction reads the same row twice and gets different values because
another transaction updated and committed the row between the reads.

### Phantom Read

A transaction runs the same condition-based query twice and gets a
different set of rows because another transaction inserted or deleted
rows that match the condition.

## 7. Comparison

  -----------------------------------------------------------------------
  Isolation level   Dirty reads       Non-repeatable    Phantom reads
                                      reads             
  ----------------- ----------------- ----------------- -----------------
  Read Uncommitted  Possible in       Possible          Possible
                    implementations                     
                    that support it                     

  Read Committed    Prevented         Possible          Possible

  Repeatable Read   Prevented         Prevented         Depends on
                                                        implementation

  Serializable      Prevented         Prevented         Prevented
  -----------------------------------------------------------------------

This table describes the standard SQL isolation-level guarantees. Actual
database behavior may be stronger than the minimum standard.

## 8. Choosing an Isolation Level

A higher isolation level generally provides stronger guarantees but may
reduce concurrency or lead to transaction retries.

For example:

-   Read Committed is often a practical default for ordinary application
    work.
-   Repeatable Read can be useful when a transaction needs a stable view
    of data.
-   Serializable is useful when correctness requires serializable
    behavior and the application can handle retries.

The right choice depends on the database, workload, and business
requirements.

## 9. Conclusion

Isolation levels define how concurrent transactions observe one
another's changes. Understanding them helps developers prevent incorrect
results when many users access the same database.

## 10. Questions to Check Understanding

1.  What is a dirty read?
2.  Which isolation level prevents dirty reads?
3.  What is the difference between a non-repeatable read and a phantom
    read?
4.  Why might a Serializable transaction need to be retried?

## 11. SQL Example: Set an Isolation Level

In PostgreSQL, you can set the isolation level for a transaction.

```sql
BEGIN TRANSACTION ISOLATION LEVEL READ COMMITTED;

SELECT *
FROM accounts
WHERE account_id = 1;

COMMIT;
```

You can also try:

```sql
BEGIN TRANSACTION ISOLATION LEVEL REPEATABLE READ;

SELECT *
FROM accounts
WHERE account_id = 1;

COMMIT;
```

Or:

```sql
BEGIN TRANSACTION ISOLATION LEVEL SERIALIZABLE;

SELECT *
FROM accounts
WHERE account_id = 1;

COMMIT;
```

To see the current transaction isolation setting in PostgreSQL:

```sql
SHOW transaction_isolation;
```

To understand the differences, open two database sessions. Start a transaction in the first session, read a row, update and commit that row in the second session, and then read it again in the first session. Repeat the experiment with different isolation levels.

PostgreSQL treats `READ UNCOMMITTED` as `READ COMMITTED`; it does not permit dirty reads.
