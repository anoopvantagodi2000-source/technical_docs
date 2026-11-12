# Database Joins

## 1. Introduction

A database often stores information in separate tables. A **JOIN**
combines related rows from two or more tables using a matching column.

For example, an application may store customer details in a `customers`
table and purchase details in an `orders` table.

## 2. Example Tables

### Customers

    customer_id name
  ------------- -------
              1 Rahul
              2 Anoop
              3 Priya

### Orders

    order_id   customer_id   amount
  ---------- ------------- --------
         101             1      500
         102             2      800
         103             1      300

The `customer_id` column connects the two tables.

## 3. INNER JOIN

An `INNER JOIN` returns only rows that have a match in both tables.

``` sql
SELECT customers.name, orders.amount
FROM customers
INNER JOIN orders
    ON customers.customer_id = orders.customer_id;
```

The result contains Rahul and Anoop because they have orders. Priya is
not included because she has no matching order.

## 4. LEFT JOIN

A `LEFT JOIN` returns every row from the left table and matching rows
from the right table.

If there is no match, columns from the right table contain `NULL`.

``` sql
SELECT customers.name, orders.amount
FROM customers
LEFT JOIN orders
    ON customers.customer_id = orders.customer_id;
```

This query includes Priya even though she has no order.

## 5. RIGHT JOIN

A `RIGHT JOIN` returns every row from the right table and matching rows
from the left table.

If there is no match, columns from the left table contain `NULL`.

``` sql
SELECT customers.name, orders.amount
FROM customers
RIGHT JOIN orders
    ON customers.customer_id = orders.customer_id;
```

In this example, every order has a matching customer, so all orders are
returned.

## 6. FULL OUTER JOIN

A `FULL OUTER JOIN` returns matching rows and also unmatched rows from
both tables. Missing values on either side are represented by `NULL`.

``` sql
SELECT customers.name, orders.amount
FROM customers
FULL OUTER JOIN orders
    ON customers.customer_id = orders.customer_id;
```

PostgreSQL supports `FULL OUTER JOIN`. MySQL does not support it
directly; it can be emulated using `UNION` with left and right joins.

## 7. How to Choose a JOIN

Ask what rows the question needs:

-   Only records that match in both tables: `INNER JOIN`.
-   Every row from the first table, whether or not it matches:
    `LEFT JOIN`.
-   Every row from the second table, whether or not it matches:
    `RIGHT JOIN`.
-   Every row from both tables: `FULL OUTER JOIN`.

## 8. Conclusion

Joins allow related information to be stored separately while still
being retrieved together. They are fundamental to querying relational
databases.



## 10. SQL Practice: Create the Example Tables

You can run the following statements to create and populate the example tables.

```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    name VARCHAR(50)
);

CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT REFERENCES customers(customer_id),
    amount DECIMAL(10, 2)
);

INSERT INTO customers (customer_id, name) VALUES
(1, 'Rahul'),
(2, 'Anoop'),
(3, 'Priya');

INSERT INTO orders (order_id, customer_id, amount) VALUES
(101, 1, 500),
(102, 2, 800),
(103, 1, 300);
```

Try these queries:

```sql
-- Customers who have placed orders
SELECT c.name, o.amount
FROM customers AS c
INNER JOIN orders AS o
    ON c.customer_id = o.customer_id;

-- All customers, including customers without orders
SELECT c.name, o.amount
FROM customers AS c
LEFT JOIN orders AS o
    ON c.customer_id = o.customer_id;
```
