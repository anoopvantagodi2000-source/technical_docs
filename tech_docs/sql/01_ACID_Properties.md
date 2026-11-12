# ACID Properties

## 1. Introduction

ACID is a set of four properties that make database transactions
reliable. ACID stands for **Atomicity, Consistency, Isolation, and
Durability**.

A transaction is a group of database operations treated as one logical
unit. For example, transferring money between two bank accounts requires
updating both accounts.

## 2. Atomicity

Atomicity means **all operations in a transaction succeed, or none of
them take effect**.

Imagine transferring ₹1,000 from Account A to Account B:

1.  Deduct ₹1,000 from Account A.
2.  Add ₹1,000 to Account B.

If the first operation succeeds but the second fails, the database must
undo the first operation. The transfer should not be left
half-completed.

**In simple terms:** A transaction is all-or-nothing.

## 3. Consistency

Consistency means a transaction must move the database from one valid
state to another valid state. Database rules and constraints must remain
satisfied.

For example, suppose an account balance must never be negative. A
transaction that would make the balance negative should be rejected.

Consistency depends on rules such as:

-   Primary key and foreign key constraints.
-   `NOT NULL` constraints.
-   `UNIQUE` constraints.
-   `CHECK` constraints.
-   Application-level business rules.

**In simple terms:** A transaction must not leave the data violating the
rules.

## 4. Isolation

Isolation means concurrent transactions should not interfere with one
another in a way that produces incorrect results.

For example, two users may try to book the same seat at nearly the same
time. The database must coordinate their operations so that the seat is
not successfully booked twice.

Databases use techniques such as locks and multiversion concurrency
control (MVCC) to manage concurrent transactions.

**In simple terms:** Transactions should behave safely when they run at
the same time.

## 5. Durability

Durability means that once a transaction is committed, its changes are
saved and should survive a database crash or restart.

For example, after a bank transfer is successfully committed, the
updated balances should remain saved even if the server restarts.

Databases commonly use transaction logs and persistent storage to
provide durability.

**In simple terms:** Committed changes should not disappear.

## 6. Summary

  -----------------------------------------------------------------------
  Property                Meaning                 Example
  ----------------------- ----------------------- -----------------------
  Atomicity               All operations happen   Both sides of a
                          or none do              transfer succeed

  Consistency             Rules remain satisfied  Account balance cannot
                                                  violate constraints

  Isolation               Concurrent transactions Two users cannot both
                          do not interfere        book the same seat
                          incorrectly             

  Durability              Committed changes       A transfer remains
                          remain saved            recorded after restart
  -----------------------------------------------------------------------

## 7. Conclusion

ACID properties help relational databases process transactions reliably.
They are especially important in systems such as banking, payments,
booking, and order management.


## 10. SQL Example: A Transaction

The following PostgreSQL example transfers money between two accounts. Both updates belong to one transaction.

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 1000
WHERE account_id = 1;

UPDATE accounts
SET balance = balance + 1000
WHERE account_id = 2;

COMMIT;
```

If the application detects an error before `COMMIT`, it can run:

```sql
ROLLBACK;
```

This example demonstrates atomicity. The database constraints and application rules help maintain consistency, while the transaction system provides isolation and durability according to the database configuration.
