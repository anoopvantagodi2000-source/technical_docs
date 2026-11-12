# CAP Theorem

## 1. Introduction

CAP theorem describes an important trade-off in distributed databases
and distributed systems.

A distributed database stores data across multiple machines, often
called **nodes**. These nodes communicate over a network.

CAP stands for:

-   **Consistency**
-   **Availability**
-   **Partition Tolerance**

The theorem is especially useful for understanding what happens when
communication between nodes fails.

## 2. Consistency

In the CAP theorem, consistency means that every read receives the
latest successful write, or an error.

For example, imagine a user changes their account email address. After
the update succeeds, a read from another node should return the new
email address, not an older value.

This is a strong consistency guarantee.

## 3. Availability

Availability means that every request sent to a functioning node
receives a response. The response may contain data that is not the
latest version.

For example, a shopping application may continue displaying product
information even when some database nodes cannot communicate with one
another.

Availability focuses on the system continuing to respond.

## 4. Partition Tolerance

A network partition occurs when nodes in a distributed system cannot
communicate with each other because of a network failure.

Partition tolerance means the system is designed to continue operating
despite this communication failure.

For example, a database may have one node in Bengaluru and another in
Mumbai. If the network connection between them breaks, the nodes may no
longer be able to exchange updates.

## 5. Understanding the CAP Trade-off

The important point is what happens **during a network partition**.

A distributed system generally has to choose between:

-   **Consistency:** Refuse or delay some requests so that it does not
    return potentially outdated data.
-   **Availability:** Continue responding to requests, even if some
    responses may contain outdated data.

It cannot guarantee both strong consistency and availability during a
partition.

Partition tolerance is not normally an optional feature in a distributed
system: network failures can happen, so systems need a way to handle
them.

## 6. Simple Example

Suppose two database nodes contain a customer's account balance.

-   Node A has a balance of ₹5,000.
-   Node B also has a balance of ₹5,000.
-   A network failure prevents the nodes from communicating.
-   A request updates the balance through Node A to ₹4,000.

Node B may not know about this update.

A consistency-focused system may reject or delay a read or write through
Node B until it can safely coordinate.

An availability-focused system may continue responding through Node B,
but it could return the old balance.

This example illustrates the trade-off; actual database behavior depends
on its design and configuration.

## 7. Important Clarification

CAP does not simply mean that every database permanently chooses only
two properties out of three.

The trade-off matters when a network partition occurs. When the network
is healthy, a system may be able to provide both consistency and
availability.

## 8. Conclusion

CAP theorem helps developers understand the behavior of distributed
databases during network failures. It is useful when choosing or
designing systems that need to balance correct, up-to-date data with
continued service availability.



## 10. SQL Example and Important Limitation

CAP is a property of a **distributed system's architecture and behavior during network partitions**. It is not a SQL command, so there is no single SQL statement that enables CAP consistency or availability.

These are ordinary SQL operations that an application might send to database nodes:

```sql
-- A write sent to the database
UPDATE product_inventory
SET quantity = quantity - 1
WHERE product_id = 101;

-- A read sent to a database
SELECT quantity
FROM product_inventory
WHERE product_id = 101;
```

In a distributed deployment, the architecture determines how writes are replicated and what happens if nodes cannot communicate. A consistency-focused design may reject or delay some requests; an availability-focused design may respond using data that could be stale.

The SQL statements alone do not determine that behavior.
