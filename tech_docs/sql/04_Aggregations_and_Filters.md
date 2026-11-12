# Aggregations and Filters in SQL

## 1. Introduction

SQL queries often need to do more than retrieve individual rows. They
may need to calculate totals, averages, counts, or filter results based
on conditions.

**Aggregation** means calculating a result from multiple rows.
**Filtering** means selecting only the rows or groups that meet a
condition.

## 2. Aggregate Functions

Common SQL aggregate functions include:

  Function    Purpose
  ----------- --------------------------------
  `COUNT()`   Counts rows or non-NULL values
  `SUM()`     Calculates a total
  `AVG()`     Calculates an average
  `MIN()`     Finds the smallest value
  `MAX()`     Finds the largest value

Example table: `orders`

    order_id   customer_id   amount
  ---------- ------------- --------
         101             1      500
         102             2      800
         103             1      300

Calculate the total amount:

``` sql
SELECT SUM(amount)
FROM orders;
```

Calculate the number of orders:

``` sql
SELECT COUNT(*)
FROM orders;
```

## 3. GROUP BY

`GROUP BY` puts rows with the same value into groups. Aggregate
functions can then calculate a result for each group.

Example: calculate the total order amount for each customer.

``` sql
SELECT customer_id, SUM(amount) AS total_amount
FROM orders
GROUP BY customer_id;
```

The database groups orders by `customer_id` and calculates one total for
each customer.

## 4. WHERE

`WHERE` filters individual rows before grouping and aggregation.

Example: find orders greater than ₹500.

``` sql
SELECT *
FROM orders
WHERE amount > 500;
```

Only rows satisfying the condition are included in the result.

## 5. HAVING

`HAVING` filters groups after `GROUP BY` has formed them.

Example: find customers whose total order amount is greater than ₹500.

``` sql
SELECT customer_id, SUM(amount) AS total_amount
FROM orders
GROUP BY customer_id
HAVING SUM(amount) > 500;
```

The database first groups orders by customer, calculates each total, and
then keeps only groups whose total is greater than 500.

## 6. WHERE vs HAVING

  -----------------------------------------------------------------------
  WHERE                               HAVING
  ----------------------------------- -----------------------------------
  Filters individual rows             Filters groups

  Applied before grouping             Applied after grouping

  Commonly used with ordinary column  Commonly used with aggregate
  conditions                          conditions
  -----------------------------------------------------------------------

A query can use both:

``` sql
SELECT customer_id, SUM(amount) AS total_amount
FROM orders
WHERE amount >= 300
GROUP BY customer_id
HAVING SUM(amount) > 500;
```

This first excludes orders below 300, then groups the remaining orders
by customer, and finally filters the groups.

## 7. A Simple Way to Read SQL Questions

When reading a question, identify:

1.  **What should be displayed?** These are usually the columns in
    `SELECT`.
2.  **Which tables contain the information?** These are used in `FROM`
    and `JOIN`.
3.  **Which individual rows should be included?** This usually requires
    `WHERE`.
4.  **Should rows be grouped?** Look for words such as per customer, per
    month, or per facility. These often indicate `GROUP BY`.
5.  **Should entire groups be filtered?** This often requires `HAVING`.
6.  **How should results be ordered?** Use `ORDER BY`.

## 8. Conclusion

Aggregate functions, `GROUP BY`, `WHERE`, and `HAVING` help summarize
and filter database information. Understanding the difference between
filtering rows and filtering groups is especially important.


## 10. SQL Practice: Create and Query an Orders Table

```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    amount DECIMAL(10, 2)
);

INSERT INTO orders (order_id, customer_id, amount) VALUES
(101, 1, 500),
(102, 2, 800),
(103, 1, 300),
(104, 2, 200),
(105, 3, 900);
```

Try these queries:

```sql
-- Count all orders
SELECT COUNT(*)
FROM orders;

-- Calculate the total amount
SELECT SUM(amount)
FROM orders;

-- Calculate each customer's total
SELECT customer_id, SUM(amount) AS total_amount
FROM orders
GROUP BY customer_id;

-- Filter individual orders before grouping
SELECT customer_id, SUM(amount) AS total_amount
FROM orders
WHERE amount >= 300
GROUP BY customer_id;

-- Filter groups after grouping
SELECT customer_id, SUM(amount) AS total_amount
FROM orders
GROUP BY customer_id
HAVING SUM(amount) > 700;
```
