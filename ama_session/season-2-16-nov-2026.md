# AMA Session – Questions and Answers


## 1.Difference Between OFFSET and LIMIT

**LIMIT:** Specifies the maximum number of records to retrieve from a table.

**OFFSET:** Specifies the number of records to skip before retrieving results.


## 2.What Are Access Modifiers?



| Modifier | Accessibility |
|---|---|
| public | Accessible from anywhere |
| protected | Accessible within the same package and subclasses |
| default | Accessible only within the same package |
| private | Accessible only within the same class |





## 3. Difference Between 2NF and 3NF

Both 2NF and 3NF are database normalization forms used to reduce data redundancy and improve data integrity.

**Second Normal Form (2NF):**
- Must satisfy 1NF.
- Must not have partial dependencies.
- Every non-prime attribute must depend on the entire candidate key, not just part of a composite candidate key.

**Third Normal Form (3NF):**
- Must satisfy 2NF.
- Must not have transitive dependencies of non-prime attributes on candidate keys.



## 4. Difference Between Python, CPython and PyPy

**Python:**

Python is a high-level programming language. It provides syntax and language specifications that developers use to write programs.

**CPython:****Key Difference:**

| Python | CPython | PyPy |
|---|---|---|
| Programming language | Python implementation | Python implementation |
| Defines language syntax and semantics | Primarily implemented in C | Implemented using RPython |
| Not an implementation itself | Uses bytecode execution | Uses JIT compilation |

---

- The standard and most widely used implementation of Python.
- Written primarily in C.
- Compiles Python source code into bytecode and executes it using a virtual machine.

**PyPy:**

- An alternative implementation of Python.
- Uses a Just-In-Time (JIT) compiler.
- Can execute certain Python programs faster than CPython.



## 5. Where Do We Use EXISTS and NOT EXISTS?

`EXISTS` and `NOT EXISTS` are SQL operators used to check whether a subquery returns any rows.

**EXISTS:**

Returns TRUE if the subquery returns at least one row.



**NOT EXISTS:**

Returns TRUE if the subquery returns no rows.


## 6. If a Child Class Does Not Implement an Abstract Method, Will the Code Run?

NO





## 7. List Five String Methods in Python

Python provides several built-in string methods.

Five commonly used methods are:

**1. upper()**

Converts all characters into uppercase.



**2. lower()**

Converts all characters into lowercase.

**3. strip()**

Removes leading and trailing whitespace.


**4. replace()**

Replaces a specified substring with another substring.



**5. split()**

Splits a string into a list based on a separator.



## 8.  Difference Between SQL and PostgreSQL

SQL is a language, whereas PostgreSQL is a database management system that uses SQL.



## 9. How to Insert Values Into a Table?

We use the `INSERT INTO` statement to insert new records into a database table.



## 10.What Are Cohesion and Coupling?


**Cohesion:**

Cohesion refers to how closely related the responsibilities of a class or module are.

- High cohesion is desirable.
- A class should focus on a single, well-defined responsibility.

**Coupling:**

Coupling refers to the degree of dependency between different classes or modules.

- Low coupling is desirable.
- Classes should depend on each other as little as possible.



## Difference Between grep and find

Both are Linux commands, but they serve different purposes.

**find:**

Used to search for files and directories based on conditions such as name, type, size, and modification time.


**grep:**

Used to search for specific text or patterns inside files.


## 12.Why Do We Use Constraints?

Constraints are rules applied to database columns or tables to maintain data integrity and consistency.



## 13.Which Query Executes First When We Use a Subquery?

A subquery is a query written inside another SQL query.



**Execution explanation:**

1. The inner query calculates the average salary.
2. The outer query retrieves employees whose salaries are greater than that average.

**Important Note:**

A correlated subquery is different. It references columns from the outer query and is logically evaluated for each relevant outer row.

Also, SQL uses declarative execution: the database optimizer determines the actual execution plan, so logical processing order is not necessarily the physical execution order.

---

## 14. How to Create a Schema in PostgreSQL?

A schema is a logical namespace inside a database. It helps organize database objects such as tables, views, functions, and sequences.

**Syntax:**

```sql
CREATE SCHEMA schema_name;
```



## 15.  What Are Window Functions?

Window functions perform calculations across a set of related rows without combining those rows into a single result.



## 16.  Difference Between Encapsulation and Abstraction

Both encapsulation and abstraction are fundamental concepts of Object-Oriented Programming (OOP).

**Encapsulation:**

Encapsulation means bundling data and methods together inside a class while controlling access to the internal data.


**Abstraction:**

Abstraction means hiding implementation details and exposing only the essential functionality.




