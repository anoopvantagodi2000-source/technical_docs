# Database Triggers

## 1. Introduction

A trigger is a database operation that runs automatically when a
specified event occurs on a table or view.

Common trigger events include:

-   `INSERT`
-   `UPDATE`
-   `DELETE`

Triggers are useful when the database needs to perform an automatic
action whenever data changes.

## 2. Why Are Triggers Used?

Imagine an application that stores employee information.

Whenever an employee's salary changes, the organization wants to record:

-   Which employee was updated.
-   The old salary.
-   The new salary.
-   When the change happened.

A trigger can automatically insert this information into an audit table.

Without a trigger, the application would need to remember to perform the
audit insert every time it updates a salary.

## 3. Example Using PostgreSQL

Suppose we have an `employees` table and a `salary_audit` table.

The audit table might contain:

    employee_id   old_salary   new_salary
  ------------- ------------ ------------
             10        40000        45000

First, create a trigger function:

``` sql
CREATE OR REPLACE FUNCTION log_salary_change()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO salary_audit
        (employee_id, old_salary, new_salary)
    VALUES
        (OLD.employee_id, OLD.salary, NEW.salary);

    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

Then create the trigger:

``` sql
CREATE TRIGGER salary_update_trigger
AFTER UPDATE OF salary ON employees
FOR EACH ROW
EXECUTE FUNCTION log_salary_change();
```

Now, when an employee's salary is updated, the trigger function inserts
the old and new salary values into the audit table.

In PostgreSQL trigger functions:

-   `OLD` refers to the previous row values.
-   `NEW` refers to the new row values.

## 4. Types of Triggers

Triggers can be classified by when they execute:

-   **BEFORE trigger:** Runs before the triggering operation is
    completed.
-   **AFTER trigger:** Runs after the triggering operation.
-   **INSTEAD OF trigger:** Runs in place of the triggering operation,
    commonly used with views where supported.

They can also be classified by how often they execute:

-   **Row-level trigger:** Runs once for each affected row.
-   **Statement-level trigger:** Runs once for the SQL statement,
    regardless of how many rows it affects.

Support and syntax vary across database systems.

## 5. Advantages of Triggers

1.  Automate database actions.
2.  Help maintain audit records.
3.  Apply certain rules close to the data.
4.  Avoid repeating the same database-side operation in multiple
    application paths.

## 6. Disadvantages of Triggers

1.  They can make application behavior less obvious.
2.  Debugging can become more difficult.
3.  Complex triggers can affect performance.
4.  A trigger may cause additional database operations that developers
    did not expect.

## 7. When Should Triggers Be Used?

Triggers can be useful for audit logging or enforcing database-level
behavior that must happen whenever a particular data change occurs.

However, they should not be used for every business rule. If important
application logic is hidden inside many triggers, the system can become
difficult to understand and maintain.

## 8. Conclusion

Triggers allow a database to automatically execute logic in response to
data changes. They are useful in selected cases, especially auditing,
but should be designed carefully.



## 10. Complete SQL Practice Example

The following PostgreSQL example creates an employee table, an audit table, a trigger function, and a trigger.

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    name VARCHAR(100),
    salary DECIMAL(10, 2)
);

CREATE TABLE salary_audit (
    audit_id SERIAL PRIMARY KEY,
    employee_id INT,
    old_salary DECIMAL(10, 2),
    new_salary DECIMAL(10, 2),
    changed_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

Insert an employee:

```sql
INSERT INTO employees (employee_id, name, salary)
VALUES (10, 'Asha', 40000);
```

Create the trigger function:

```sql
CREATE OR REPLACE FUNCTION log_salary_change()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO salary_audit (employee_id, old_salary, new_salary)
    VALUES (OLD.employee_id, OLD.salary, NEW.salary);

    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

Create the trigger:

```sql
CREATE TRIGGER salary_update_trigger
AFTER UPDATE OF salary ON employees
FOR EACH ROW
EXECUTE FUNCTION log_salary_change();
```

Update the salary:

```sql
UPDATE employees
SET salary = 45000
WHERE employee_id = 10;
```

Check the audit record:

```sql
SELECT *
FROM salary_audit;
```

You should see the previous salary (40000) and the new salary (45000) in the audit table.
