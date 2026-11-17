# SQL Cheat Sheet

A practical reference for SQL and PostgreSQL, covering database setup,
user management, cleanup, querying, joins, aggregation, subqueries,
window functions, and transactions.

**Database:** PostgreSQL

------------------------------------------------------------------------

## 1. SQL Command Categories

  -----------------------------------------------------------------------
  Category                Purpose                 Commands
  ----------------------- ----------------------- -----------------------
  DDL                     Define database         `CREATE`, `ALTER`,
                          structure               `DROP`, `TRUNCATE`

  DML                     Modify records          `INSERT`, `UPDATE`,
                                                  `DELETE`

  DQL                     Retrieve records        `SELECT`

  DCL                     Manage permissions      `GRANT`, `REVOKE`

  TCL                     Manage transactions     `COMMIT`, `ROLLBACK`,
                                                  `SAVEPOINT`
  -----------------------------------------------------------------------

## 2. Database Operations

``` sql
-- Create database
CREATE DATABASE company;

-- Connect to database (psql)
\c company

-- List databases
\l

-- List tables
\dt

-- Describe table
\d employees

-- Show current database
SELECT current_database();

-- Delete database
DROP DATABASE company;
```

## 3. Database and User Management

### Create a database

``` sql
CREATE DATABASE company;

-- Create database with a specific owner
CREATE DATABASE company OWNER anoop;
```

### Create users and roles

In PostgreSQL, a user is a role with the `LOGIN` attribute.

``` sql
-- Create user
CREATE USER anoop WITH PASSWORD 'your_password';

-- Create role
CREATE ROLE developer;

-- Create a login role
CREATE ROLE app_user WITH LOGIN PASSWORD 'your_password';

-- List users and roles (psql)
\du
```

### Grant permissions

``` sql
-- Allow user to connect to database
GRANT CONNECT ON DATABASE company TO anoop;

-- Allow access to schema
GRANT USAGE ON SCHEMA public TO anoop;

-- Grant permissions on existing tables
GRANT SELECT, INSERT, UPDATE, DELETE
ON ALL TABLES IN SCHEMA public
TO anoop;

-- Grant all privileges on existing tables
GRANT ALL PRIVILEGES
ON ALL TABLES IN SCHEMA public
TO anoop;

-- Grant permissions on future tables created by the role
ALTER DEFAULT PRIVILEGES FOR ROLE anoop IN SCHEMA public
GRANT SELECT, INSERT, UPDATE, DELETE ON TABLES TO anoop;

-- Change database owner
ALTER DATABASE company OWNER TO anoop;
```

**Note:** Default privileges apply to objects created in the future by
the specified role. They do not automatically apply to objects created
by other users.

### Revoke permissions

``` sql
REVOKE ALL PRIVILEGES
ON ALL TABLES IN SCHEMA public
FROM anoop;

REVOKE CONNECT ON DATABASE company FROM anoop;
```

### Change password

``` sql
ALTER USER anoop WITH PASSWORD 'new_password';

-- Safer interactive option in psql
\password anoop
```

### Drop a user or role

``` sql
DROP USER IF EXISTS anoop;
DROP ROLE IF EXISTS developer;
```

A role cannot be dropped while it owns objects or has dependent
privileges. Reassign or remove those dependencies first.

## 4. Database Cleanup and Reset

Use these commands carefully. They permanently remove data or database
objects.

### Delete rows or reset a table

``` sql
-- Delete selected rows
DELETE FROM employees WHERE id = 1;

-- Delete all rows
DELETE FROM employees;

-- Remove all rows efficiently
TRUNCATE TABLE employees;

-- Remove all rows and restart identity sequences
TRUNCATE TABLE employees RESTART IDENTITY;
```

### Drop tables and schemas

``` sql
DROP TABLE IF EXISTS employees;

-- Drop schema and dependent objects
DROP SCHEMA IF EXISTS my_schema CASCADE;

-- Recreate schema
CREATE SCHEMA my_schema;
```

`CASCADE` also removes dependent objects. Use it only when you
understand the dependencies.

### Complete database reset

Run this while connected to a different database, such as `postgres` ---
not to the database being dropped.

``` sql
-- STEP 1: Terminate other sessions connected to the target database
SELECT pg_terminate_backend(pid)
FROM pg_stat_activity
WHERE datname = 'unpopulation'
  AND pid <> pg_backend_pid();

-- STEP 2: Drop the database
DROP DATABASE IF EXISTS unpopulation;

-- STEP 3: Drop the user
DROP USER IF EXISTS anoop;
```

**Important:** - You need sufficient privileges to terminate other
sessions and drop the database. - Terminating sessions interrupts
connected clients and may roll back their uncommitted work. -
`DROP USER` can fail if the role owns objects or has dependent
privileges. - `DROP OWNED BY anoop` can remove objects owned by that
role and revoke its privileges in the current database. It is
destructive and must be considered across every database where the role
has dependencies. - `DROP DATABASE` cannot run inside a transaction
block.

## 5. PostgreSQL CLI (`psql`) Commands

These are `psql` meta-commands, not SQL statements.

  Command                      Description
  ---------------------------- -------------------------------
  `psql -U postgres`           Connect as postgres user
  `psql -U anoop -d company`   Connect to company database
  `\l`                         List databases
  `\c company`                 Connect to database
  `\dt`                        List tables
  `\d employees`               Describe table
  `\du`                        List users and roles
  `\dn`                        List schemas
  `\conninfo`                  Show connection details
  `\q`                         Exit PostgreSQL
  `\h SELECT`                  Help for SQL command
  `\?`                         Help for psql commands
  `\password anoop`            Change password interactively

## 6. Table Operations (DDL)

### Create a table

``` sql
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(150) UNIQUE,
    salary NUMERIC(10, 2),
    department_id INT,
    joining_date DATE DEFAULT CURRENT_DATE
);
```

### Alter a table

``` sql
-- Add column
ALTER TABLE employees ADD COLUMN age INT;

-- Change column type
ALTER TABLE employees
ALTER COLUMN salary TYPE NUMERIC(12, 2);

-- Rename column
ALTER TABLE employees RENAME COLUMN name TO full_name;

-- Drop column
ALTER TABLE employees DROP COLUMN age;
```

### DELETE vs TRUNCATE vs DROP

  Command      What it does
  ------------ ----------------------------------------
  `DELETE`     Removes selected rows; can use `WHERE`
  `TRUNCATE`   Removes all rows from a table
  `DROP`       Removes the table itself

## 7. SELECT -- Retrieve Data

``` sql
-- All columns
SELECT * FROM employees;

-- Specific columns
SELECT name, salary FROM employees;

-- Unique values
SELECT DISTINCT department_id FROM employees;

-- Column alias
SELECT name AS employee_name FROM employees;

-- Calculated column
SELECT name, salary * 12 AS annual_salary
FROM employees;
```

## 8. Filtering -- WHERE

``` sql
SELECT * FROM employees
WHERE salary > 50000;

-- Multiple conditions
SELECT * FROM employees
WHERE salary > 50000 AND department_id = 10;

-- OR
SELECT * FROM employees
WHERE department_id = 10 OR department_id = 20;

-- NOT
SELECT * FROM employees
WHERE NOT department_id = 10;

-- BETWEEN (inclusive)
SELECT * FROM employees
WHERE salary BETWEEN 30000 AND 60000;

-- IN
SELECT * FROM employees
WHERE department_id IN (10, 20, 30);

-- LIKE
SELECT * FROM employees
WHERE name LIKE 'A%';

-- Case-insensitive matching (PostgreSQL)
SELECT * FROM employees
WHERE name ILIKE 'a%';

-- NULL checking
SELECT * FROM employees
WHERE department_id IS NULL;
```

### LIKE wildcards

  Pattern    Meaning
  ---------- -------------------------------------
  `'A%'`     Starts with A
  `'%A'`     Ends with A
  `'%A%'`    Contains A
  `'A_'`     A followed by exactly one character
  `'__A%'`   Third character is A

Use `IS NULL` and `IS NOT NULL`, not `= NULL`.

## 9. Sorting and Pagination

``` sql
-- Ascending
SELECT * FROM employees ORDER BY salary ASC;

-- Descending
SELECT * FROM employees ORDER BY salary DESC;

-- Multiple columns
SELECT * FROM employees
ORDER BY department_id ASC, salary DESC;

-- First 5 rows
SELECT * FROM employees LIMIT 5;

-- Skip first 5 rows
SELECT * FROM employees OFFSET 5;

-- Second page, 10 rows per page
SELECT * FROM employees
ORDER BY id
LIMIT 10 OFFSET 10;
```

Pagination formula:

`OFFSET = (page_number - 1) * page_size`

Use `ORDER BY` with pagination to make results deterministic.

## 10. Aggregate Functions

  Function    Purpose
  ----------- --------------------------------
  `COUNT()`   Counts rows or non-NULL values
  `SUM()`     Calculates total
  `AVG()`     Calculates average
  `MIN()`     Finds minimum
  `MAX()`     Finds maximum

``` sql
SELECT COUNT(*) FROM employees;
SELECT SUM(salary) FROM employees;
SELECT AVG(salary) FROM employees;
SELECT MIN(salary), MAX(salary) FROM employees;
```

``` sql
COUNT(*)                       -- Counts all rows
COUNT(salary)                  -- Counts non-NULL salary values
COUNT(DISTINCT department_id)  -- Counts unique, non-NULL departments
```

## 11. GROUP BY

Groups rows that share the same values.

``` sql
-- Average salary per department
SELECT department_id, AVG(salary) AS average_salary
FROM employees
GROUP BY department_id;

-- Employee count per department
SELECT department_id, COUNT(*) AS employee_count
FROM employees
GROUP BY department_id;
```

Every selected column must either be included in `GROUP BY` or be used
inside an aggregate function.

## 12. HAVING vs WHERE

  -----------------------------------------------------------------------
  WHERE                               HAVING
  ----------------------------------- -----------------------------------
  Filters individual rows             Filters groups

  Applied before grouping             Applied after grouping

  Usually filters non-aggregated      Commonly filters aggregate results
  values                              
  -----------------------------------------------------------------------

``` sql
-- Departments with more than 5 employees
SELECT department_id, COUNT(*) AS employee_count
FROM employees
GROUP BY department_id
HAVING COUNT(*) > 5;

-- Departments with average salary above 50,000
SELECT department_id, AVG(salary)
FROM employees
GROUP BY department_id
HAVING AVG(salary) > 50000;
```

## 13. JOINS

Joins combine rows from multiple tables using related columns.

Assume `employees.department_id` references `departments.id`.

### INNER JOIN

Returns only matching rows.

``` sql
SELECT e.name, d.department_name
FROM employees e
INNER JOIN departments d
ON e.department_id = d.id;
```

### LEFT JOIN

Returns every row from the left table and matching rows from the right
table. Unmatched right-side columns are `NULL`.

``` sql
SELECT e.name, d.department_name
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.id;
```

### RIGHT JOIN

Returns every row from the right table and matching rows from the left
table.

``` sql
SELECT e.name, d.department_name
FROM employees e
RIGHT JOIN departments d
ON e.department_id = d.id;
```

### FULL OUTER JOIN

Returns matching and non-matching rows from both tables.

``` sql
SELECT e.name, d.department_name
FROM employees e
FULL OUTER JOIN departments d
ON e.department_id = d.id;
```

### CROSS JOIN

Returns every possible combination of rows.

``` sql
SELECT e.name, d.department_name
FROM employees e
CROSS JOIN departments d;
```

### SELF JOIN

Joins a table with itself.

``` sql
SELECT e.name AS employee, m.name AS manager
FROM employees e
LEFT JOIN employees m
ON e.manager_id = m.id;
```

  Join    Result
  ------- -------------------------------------
  INNER   Matching rows only
  LEFT    All left rows + matching right rows
  RIGHT   All right rows + matching left rows
  FULL    All rows from both sides
  CROSS   Every possible combination
  SELF    A table joined with itself

## 14. Subqueries

A subquery is a query nested inside another query.

### IN

``` sql
SELECT *
FROM employees
WHERE department_id IN (
    SELECT id
    FROM departments
    WHERE department_name IN ('Engineering', 'HR')
);
```

### EXISTS

Find departments that have at least one employee.

``` sql
SELECT *
FROM departments d
WHERE EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.id
);
```

### NOT EXISTS

Find departments without employees.

``` sql
SELECT *
FROM departments d
WHERE NOT EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.id
);
```

### Scalar subquery

Find employees earning more than the company average.

``` sql
SELECT *
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

  -----------------------------------------------------------------------
  `IN`                                `EXISTS`
  ----------------------------------- -----------------------------------
  Checks whether a value matches a    Checks whether a matching row
  set                                 exists

  Compares values                     Tests row existence

  Useful for membership checks        Useful for correlated existence
                                      checks
  -----------------------------------------------------------------------

Be careful with `NOT IN` if the subquery can return `NULL`. `NOT EXISTS`
is often safer.

## 15. Common Table Expressions (CTEs)

CTEs create named query results using `WITH`.

``` sql
WITH high_salary AS (
    SELECT *
    FROM employees
    WHERE salary > 50000
)
SELECT *
FROM high_salary;
```

### Multiple CTEs

``` sql
WITH department_count AS (
    SELECT department_id, COUNT(*) AS total
    FROM employees
    GROUP BY department_id
),
high_count AS (
    SELECT *
    FROM department_count
    WHERE total > 5
)
SELECT *
FROM high_count;
```

## 16. Set Operations

Set operations combine the results of multiple `SELECT` statements.

``` sql
-- UNION: combines results and removes duplicates
SELECT name FROM employees
UNION
SELECT name FROM managers;

-- UNION ALL: keeps duplicates
SELECT name FROM employees
UNION ALL
SELECT name FROM managers;

-- INTERSECT: returns common rows
SELECT name FROM employees
INTERSECT
SELECT name FROM managers;

-- EXCEPT: rows in the first result but not the second
SELECT name FROM employees
EXCEPT
SELECT name FROM managers;
```

Both queries must return the same number of columns with compatible data
types.

## 17. CASE Expression

Adds conditional logic to a query.

``` sql
SELECT
    name,
    salary,
    CASE
        WHEN salary >= 100000 THEN 'High'
        WHEN salary >= 50000 THEN 'Medium'
        ELSE 'Low'
    END AS salary_category
FROM employees;
```

## 18. Window Functions

Window functions calculate values across related rows without collapsing
them into one row per group.

### ROW_NUMBER

``` sql
SELECT
    name,
    salary,
    ROW_NUMBER() OVER (ORDER BY salary DESC) AS row_num
FROM employees;
```

### RANK

Ties receive the same rank, and the next rank has a gap.

``` sql
SELECT
    name,
    salary,
    RANK() OVER (ORDER BY salary DESC) AS salary_rank
FROM employees;
```

### DENSE_RANK

Ties receive the same rank, but the next rank has no gap.

``` sql
SELECT
    name,
    salary,
    DENSE_RANK() OVER (ORDER BY salary DESC) AS salary_rank
FROM employees;
```

### PARTITION BY

Ranks employees separately within each department.

``` sql
SELECT
    name,
    department_id,
    salary,
    RANK() OVER (
        PARTITION BY department_id
        ORDER BY salary DESC
    ) AS department_rank
FROM employees;
```

### LAG and LEAD

Access values from a previous or following row.

``` sql
SELECT
    name,
    salary,
    LAG(salary) OVER (ORDER BY id) AS previous_salary,
    LEAD(salary) OVER (ORDER BY id) AS next_salary
FROM employees;
```

## 19. GROUPING SETS, ROLLUP and CUBE

These features calculate multiple levels of aggregation.

### GROUPING SETS

Explicitly specifies grouping combinations.

``` sql
SELECT department_id, joining_date, COUNT(*)
FROM employees
GROUP BY GROUPING SETS (
    (department_id),
    (joining_date),
    (department_id, joining_date),
    ()
);
```

### ROLLUP

Creates hierarchical subtotals and a grand total.

``` sql
SELECT department_id, joining_date, COUNT(*)
FROM employees
GROUP BY ROLLUP (department_id, joining_date);
```

For two columns, `ROLLUP` produces: 1. Department and joining-date
groups. 2. Department subtotals. 3. Grand total.

### CUBE

Creates subtotals for every possible combination.

``` sql
SELECT department_id, joining_date, COUNT(*)
FROM employees
GROUP BY CUBE (department_id, joining_date);
```

## 20. Constraints

Constraints enforce data integrity.

  Constraint    Purpose
  ------------- ---------------------------
  PRIMARY KEY   Uniquely identifies a row
  FOREIGN KEY   References another table
  UNIQUE        Prevents duplicate values
  NOT NULL      Prevents NULL values
  CHECK         Validates a condition
  DEFAULT       Supplies a default value

``` sql
CREATE TABLE departments (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) UNIQUE NOT NULL
);

CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    salary NUMERIC CHECK (salary >= 0),
    department_id INT REFERENCES departments(id)
);
```

## 21. INSERT, UPDATE and DELETE

### INSERT

``` sql
INSERT INTO employees (name, salary, department_id)
VALUES ('Anoop', 60000, 10);

-- Insert multiple rows
INSERT INTO employees (name, salary)
VALUES
    ('Rahul', 50000),
    ('John', 70000);
```

### UPDATE

``` sql
UPDATE employees
SET salary = 65000
WHERE id = 1;
```

### DELETE

``` sql
DELETE FROM employees
WHERE id = 1;
```

**Warning:** Verify your `WHERE` condition before executing `UPDATE` or
`DELETE`. Without a `WHERE` clause, all rows are affected.

## 22. Transactions

Transactions make a group of operations succeed or fail together.

``` sql
BEGIN;

UPDATE accounts
SET balance = balance - 1000
WHERE id = 1;

UPDATE accounts
SET balance = balance + 1000
WHERE id = 2;

COMMIT;
```

If something goes wrong:

``` sql
ROLLBACK;
```

### ACID properties

  Property      Meaning
  ------------- --------------------------------------
  Atomicity     All operations succeed or none do
  Consistency   Database remains valid
  Isolation     Concurrent transactions are isolated
  Durability    Committed changes persist

## 23. Indexes

Indexes can improve data retrieval performance.

``` sql
-- Create index
CREATE INDEX idx_employee_name
ON employees(name);

-- Create unique index
CREATE UNIQUE INDEX idx_employee_email
ON employees(email);

-- Drop index
DROP INDEX idx_employee_name;
```

Remember: - Indexes can speed up suitable `SELECT` queries. - Indexes
consume storage. - Indexes add overhead to `INSERT`, `UPDATE`, and
`DELETE`. - Choose indexes based on actual query patterns and execution
plans.

## 24. SQL Logical Query Processing Order

SQL is written with `SELECT` first, but its logical processing order is
approximately:

1.  `FROM`
2.  `JOIN` / `ON`
3.  `WHERE`
4.  `GROUP BY`
5.  `HAVING`
6.  `SELECT`
7.  `DISTINCT`
8.  `ORDER BY`
9.  `LIMIT` / `OFFSET`

Example:

``` sql
SELECT department_id, AVG(salary) AS avg_salary
FROM employees
WHERE salary > 20000
GROUP BY department_id
HAVING AVG(salary) > 50000
ORDER BY avg_salary DESC
LIMIT 5;
```

This order helps explain: - Why `WHERE` cannot filter aggregate
results. - Why `HAVING` is used after grouping. - Why a `SELECT` alias
generally cannot be used in `WHERE`. - Why `LIMIT` is applied after
sorting.

## 25. Common SQL Mistakes

  -----------------------------------------------------------------------
  Mistake                             Correct approach
  ----------------------------------- -----------------------------------
  `column = NULL`                     `column IS NULL`

  Using aggregate in `WHERE`          Use `HAVING` or a subquery

  Selecting ungrouped columns         Group or aggregate them

  Forgetting a join condition         Specify the `ON` condition

  Using `NOT IN` with possible NULL   Consider `NOT EXISTS`
  values                              

  Using `LIMIT` without `ORDER BY`    Add deterministic ordering

  Using `COUNT(column)` to count all  Use `COUNT(*)`
  rows                                

  Confusing LEFT JOIN and INNER JOIN  Check which unmatched rows are
                                      retained
  -----------------------------------------------------------------------

## 26. Quick Revision: Which SQL Concept Should I Use?

  Requirement                     SQL concept
  ------------------------------- ----------------------------------
  Retrieve data                   `SELECT`
  Remove duplicate results        `DISTINCT`
  Filter individual rows          `WHERE`
  Sort records                    `ORDER BY`
  Limit records                   `LIMIT`
  Skip records                    `OFFSET`
  Group similar records           `GROUP BY`
  Filter grouped results          `HAVING`
  Combine related tables          `JOIN`
  Check whether a record exists   `EXISTS`
  Check values against a list     `IN`
  Create named query blocks       `CTE`
  Assign rankings                 `RANK` / `DENSE_RANK`
  Calculate across rows           Window functions
  Combine query results           `UNION` / `INTERSECT` / `EXCEPT`
  Generate subtotals              `ROLLUP` / `CUBE`
  Create database                 `CREATE DATABASE`
  Create a database user          `CREATE USER`
  Grant access                    `GRANT`
  Remove database                 `DROP DATABASE`
  Reset table data                `TRUNCATE`

## 27. Recommended Practice

Practice SQL in this order:

1.  `SELECT`, `WHERE`, `DISTINCT`
2.  `ORDER BY`, `LIMIT`, `OFFSET`
3.  Aggregate functions
4.  `GROUP BY` and `HAVING`
5.  `INNER JOIN` and `LEFT JOIN`
6.  Multiple joins
7.  Subqueries
8.  `EXISTS` and `NOT EXISTS`
9.  CTEs
10. Set operations
11. Window functions
12. `GROUPING SETS`, `ROLLUP`, and `CUBE`
13. Indexes and query optimization

### Practice resources

-   [SQLBolt](https://sqlbolt.com/) -- Interactive SQL lessons.
-   [PGExercises](https://pgexercises.com/) -- Practical PostgreSQL
    exercises.
-   [HackerRank SQL](https://www.hackerrank.com/domains/sql) -- SQL
    challenges.
-   [LeetCode Database](https://leetcode.com/problemset/database/) --
    SQL interview problems.
-   [PostgreSQL Documentation](https://www.postgresql.org/docs/current/)
    -- Official reference.

------------------------------------------------------------------------

**Note:** This cheat sheet uses PostgreSQL syntax. Some commands and
functions differ in MySQL, SQL Server, and SQLite.
