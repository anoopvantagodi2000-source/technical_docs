# Database Indexes

## 1. Introduction

An index is a data structure that helps a database find rows more
efficiently.

Imagine looking for a topic in a large book. Without an index, you might
have to inspect every page. With an index, you can find the relevant
page much faster.

A database index serves a similar purpose: it helps the database locate
rows without always scanning the entire table.

## 2. Example Without an Index

Suppose a `users` table contains one million records.

``` sql
SELECT *
FROM users
WHERE email = 'anoop@example.com';
```

If there is no suitable index, the database may perform a sequential
scan: it checks rows until it finds the matching email or reaches the
end of the table.

The exact execution plan depends on the database and query.

## 3. Creating an Index

An index can be created on the `email` column:

``` sql
CREATE INDEX idx_users_email
ON users(email);
```

The database can use this index to locate rows matching an email more
efficiently.

A query does not need a special syntax to use an index. The query
planner decides whether using the index is beneficial.

## 4. Common Types of Indexes

Different databases support different index types. Common examples
include:

-   **B-tree index:** A general-purpose index commonly used for equality
    and range searches.
-   **Hash index:** Designed primarily for equality comparisons in
    databases that support it.
-   **Composite index:** An index built using more than one column.
-   **Unique index:** Ensures that indexed values are unique.

For example, a composite index can be created like this:

``` sql
CREATE INDEX idx_orders_customer_date
ON orders(customer_id, order_date);
```

The order of columns in a composite index matters. The index is most
useful for queries that match its leading columns.

## 5. Advantages of Indexes

1.  Can make data retrieval faster.
2.  Can improve queries using `WHERE`.
3.  Can help with joins when suitable join columns are indexed.
4.  Can help with sorting and range queries.

## 6. Disadvantages of Indexes

1.  Indexes use additional disk space.
2.  Inserts, updates, and deletes may become slower because indexes must
    be maintained.
3.  Too many or poorly chosen indexes can increase storage and
    maintenance costs.
4.  An index does not guarantee that every query will become faster.

## 7. How to Check Whether an Index Is Useful

Use the query execution plan.

In PostgreSQL:

``` sql
EXPLAIN
SELECT *
FROM users
WHERE email = 'anoop@example.com';
```

`EXPLAIN` shows the plan the database intends to use. `EXPLAIN ANALYZE`
actually runs the query and reports measured execution details.

## 8. Conclusion

Indexes are an important database performance tool. They should be
created based on real query patterns rather than added to every column
automatically.



## 10. SQL Example: Create and Inspect an Index

Create a table and add sample data:

```sql
CREATE TABLE users (
    user_id INT PRIMARY KEY,
    email VARCHAR(200) NOT NULL,
    full_name VARCHAR(100)
);

INSERT INTO users (user_id, email, full_name) VALUES
(1, 'anoop@example.com', 'Anoop'),
(2, 'rahul@example.com', 'Rahul'),
(3, 'priya@example.com', 'Priya');
```

Create an index on the column used for searching:

```sql
CREATE INDEX idx_users_email
ON users(email);
```

Run a query that can use the index:

```sql
SELECT *
FROM users
WHERE email = 'anoop@example.com';
```

Inspect the query plan in PostgreSQL:

```sql
EXPLAIN
SELECT *
FROM users
WHERE email = 'anoop@example.com';
```

For a very small table, PostgreSQL may choose a sequential scan because it can be cheaper. An index does not guarantee that the query planner will use it.
