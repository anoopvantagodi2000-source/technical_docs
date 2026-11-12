# Database Normalization

## 1. Introduction

Normalization is the process of organizing data into related tables to
reduce unnecessary duplication and improve data consistency.

Consider a table that stores customer details together with their
orders.

    customer_id customer_name     order_id product
  ------------- --------------- ---------- ----------
              1 Rahul                  101 Laptop
              1 Rahul                  102 Mouse
              2 Priya                  103 Keyboard

Rahul's name is repeated because he has placed more than one order.

If his name changes, multiple rows may need to be updated. If one row is
missed, the database may contain inconsistent information.

Normalization helps organize this information into separate tables.

## 2. Why Is Normalization Needed?

Normalization helps reduce three common data problems, called anomalies:

-   **Update anomaly:** The same information is stored in several
    places, and an update may not be applied everywhere.
-   **Insertion anomaly:** It may be difficult to add one kind of
    information without adding unrelated information.
-   **Deletion anomaly:** Deleting one record may accidentally remove
    information that should have been kept.

## 3. First Normal Form (1NF)

A table is in First Normal Form when:

-   Each column contains a single, atomic value.
-   There are no repeating groups or lists stored in a single cell.
-   Rows can be uniquely identified.

For example, storing `"9876, 1234"` in one phone-number column is not a
good 1NF design if the application needs to treat each phone number as a
separate value.

A better design stores one phone number per row in a related table.

## 4. Second Normal Form (2NF)

A table is in Second Normal Form when:

-   It is already in 1NF.
-   Every non-key column depends on the whole candidate key, not just
    part of a composite candidate key.

This is mainly relevant when a table uses a composite key (a key made
from more than one column).

For example, consider a table with a composite key of
`(student_id, course_id)`. If `student_name` depends only on
`student_id`, it does not depend on the whole key. Student details
should be stored separately.

## 5. Third Normal Form (3NF)

A table is in Third Normal Form when:

-   It is already in 2NF.
-   Non-key attributes do not depend on other non-key attributes.

For example, suppose an employee table stores:

-   `employee_id`
-   `department_id`
-   `department_name`

If `department_name` depends on `department_id`, rather than directly on
`employee_id`, department details should generally be stored in a
separate department table.

## 6. Example of a More Normalized Design

Instead of repeating customer details in every order, use separate
tables.

### Customers

    customer_id customer_name
  ------------- ---------------
              1 Rahul
              2 Priya

### Orders

    order_id   customer_id product
  ---------- ------------- ----------
         101             1 Laptop
         102             1 Mouse
         103             2 Keyboard

The `customer_id` in the Orders table refers to the customer in the
Customers table.

This reduces repeated customer information and makes updates easier.

## 7. Advantages and Trade-offs

Advantages:

1.  Reduces unnecessary duplication.
2.  Helps maintain consistent data.
3.  Makes updates and maintenance easier.
4.  Helps prevent insertion, update, and deletion anomalies.

Trade-off:

Highly normalized designs may require more joins to retrieve
information. In some systems, developers deliberately use
denormalization for performance or reporting reasons.

## 8. Conclusion

Normalization is an important part of relational database design. It
helps organize information into meaningful tables and reduces problems
caused by duplicated data.


## 10. SQL Example: Separating Customer and Order Data

A normalized design can keep customer information and order information in separate tables.

```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL
);

CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT NOT NULL,
    product_name VARCHAR(100) NOT NULL,
    FOREIGN KEY (customer_id)
        REFERENCES customers(customer_id)
);
```

Insert the customer once, then refer to that customer from multiple orders:

```sql
INSERT INTO customers (customer_id, customer_name)
VALUES (1, 'Rahul');

INSERT INTO orders (order_id, customer_id, product_name)
VALUES
(101, 1, 'Laptop'),
(102, 1, 'Mouse');
```

The customer name is stored once instead of being repeated in every order row.

Retrieve the combined information using a join:

```sql
SELECT c.customer_name, o.order_id, o.product_name
FROM customers AS c
JOIN orders AS o
    ON c.customer_id = o.customer_id;
```
