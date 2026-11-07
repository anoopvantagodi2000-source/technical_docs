# SOLID Principles in Python

## Introduction

SOLID is a set of five object-oriented design principles that help us write maintainable, flexible, and reusable code.

These principles help us manage changes in software without unnecessarily affecting other parts of the application.

The five principles are:

1. **S – Single Responsibility Principle (SRP)**
2. **O – Open/Closed Principle (OCP)**
3. **L – Liskov Substitution Principle (LSP)**
4. **I – Interface Segregation Principle (ISP)**
5. **D – Dependency Inversion Principle (DIP)**

---

## 1. Single Responsibility Principle (SRP)

### What is it?

A class should have one responsibility and one main reason to change.

### Why do we need it?

When a class handles multiple responsibilities, changes in one responsibility can affect other responsibilities.

This makes the code difficult to maintain and test.

### Example: Without SRP

```python
class Order:
    def calculate_total(self, items):
        return sum(items)

    def save_order(self, order):
        print("Saving order to database")

    def send_email(self, email):
        print("Sending confirmation email")
```

The `Order` class handles three responsibilities:
- Calculating the order total.
- Saving the order.
- Sending email notifications.

### Improved design

Separate responsibilities into different classes.

```python
class OrderCalculator:
    def calculate_total(self, items):
        return sum(items)


class OrderRepository:
    def save_order(self, order):
        print("Saving order to database")


class EmailService:
    def send_email(self, email):
        print("Sending confirmation email")
```

Now each class has a separate responsibility.

**Key takeaway:** Keep responsibilities separate so that a change in one area does not unnecessarily affect another area.

---

## 2. Open/Closed Principle (OCP)

### What is it?

Software components should be open for extension but closed for modification.

In simple terms, we should be able to add new functionality without repeatedly modifying existing working code.

### Why do we need it?

When adding new features requires changing existing code, we increase the risk of breaking functionality that already works.

### Example: Without OCP

```python
class Payment:
    def process_payment(self, payment_type):
        if payment_type == "razorpay":
            print("Processing Razorpay payment")

        elif payment_type == "stripe":
            print("Processing Stripe payment")
```

Every time we introduce a new payment provider, we must modify this class.

### Improved design

```python
from abc import ABC, abstractmethod

class Payment(ABC):

    @abstractmethod
    def pay(self):
        pass


class Razorpay(Payment):
    def pay(self):
        print("Processing Razorpay payment")


class Stripe(Payment):
    def pay(self):
        print("Processing Stripe payment")


class PayPal(Payment):
    def pay(self):
        print("Processing PayPal payment")
```

Now we can introduce new payment providers by creating new classes that follow the `Payment` contract.

**Key takeaway:** Add new functionality by extending existing designs instead of repeatedly modifying stable code.

---

## 3. Liskov Substitution Principle (LSP)

### What is it?

Objects of a child class should be usable wherever an object of its parent class is expected without breaking the expected behaviour.

### Why do we need it?

Inheritance allows child classes to reuse parent functionality.

However, a child class should not violate the behaviour promised by its parent class.

### Example: Without LSP

```python
class Bird:

    def fly(self):
        print("Flying")


class Penguin(Bird):

    def fly(self):
        raise Exception("Penguins cannot fly")
```

The `Penguin` class inherits from `Bird`, but it cannot satisfy the expected behaviour of `Bird`.

A function expecting every bird to fly will fail when it receives a penguin.

### Improved design

Separate flying behaviour from general bird behaviour.

```python
class Bird:
    pass


class FlyingBird(Bird):

    def fly(self):
        print("Flying")


class Sparrow(FlyingBird):
    pass


class Penguin(Bird):
    pass
```

Now we only expect flying behaviour from birds that can actually fly.

**Key takeaway:** A child class should respect the expected behaviour of its parent class.

---

## 4. Interface Segregation Principle (ISP)

### What is it?

A class should not be forced to implement methods that it does not need.

### Why do we need it?

Large interfaces often contain methods that are irrelevant to some implementing classes.

This creates unnecessary dependencies and forces classes to provide meaningless implementations.

### Example: Without ISP

```python
from abc import ABC, abstractmethod

class Machine(ABC):

    @abstractmethod
    def print_document(self):
        pass

    @abstractmethod
    def scan_document(self):
        pass

    @abstractmethod
    def fax_document(self):
        pass


class BasicPrinter(Machine):

    def print_document(self):
        print("Printing document")

    def scan_document(self):
        raise NotImplementedError

    def fax_document(self):
        raise NotImplementedError
```

A basic printer only needs printing functionality, but it is forced to implement scanning and faxing methods.

### Improved design

Separate the interfaces based on their responsibilities.

```python
from abc import ABC, abstractmethod

class Printer(ABC):

    @abstractmethod
    def print_document(self):
        pass


class Scanner(ABC):

    @abstractmethod
    def scan_document(self):
        pass


class BasicPrinter(Printer):

    def print_document(self):
        print("Printing document")


class MultiFunctionPrinter(Printer, Scanner):

    def print_document(self):
        print("Printing document")

    def scan_document(self):
        print("Scanning document")
```

Now classes only implement the functionality they actually need.

**Key takeaway:** Keep interfaces small and focused. Do not force classes to depend on unnecessary methods.

---

## 5. Dependency Inversion Principle (DIP)

### What is it?

High-level modules should not depend directly on low-level modules.

Both should depend on abstractions.

In simple terms, business logic should not be tightly connected to a specific implementation.

### Why do we need it?

When high-level business logic directly depends on a concrete implementation, replacing that implementation may require modifying the business logic.

### Example: Without DIP

```python
class MySQLDatabase:

    def save(self, data):
        print("Saving data to MySQL")


class OrderService:

    def __init__(self):
        self.database = MySQLDatabase()

    def save_order(self, order):
        self.database.save(order)
```

`OrderService` directly creates and depends on `MySQLDatabase`.

If we replace MySQL with PostgreSQL, we need to modify `OrderService`.

### Improved design

Introduce an abstraction.

```python
from abc import ABC, abstractmethod

class Database(ABC):

    @abstractmethod
    def save(self, data):
        pass


class MySQLDatabase(Database):

    def save(self, data):
        print("Saving data to MySQL")


class PostgreSQLDatabase(Database):

    def save(self, data):
        print("Saving data to PostgreSQL")


class OrderService:

    def __init__(self, database: Database):
        self.database = database

    def save_order(self, order):
        self.database.save(order)
```

Usage:

```python
mysql = MySQLDatabase()
order_service = OrderService(mysql)

order_service.save_order("Order 101")
```

We can also use PostgreSQL:

```python
postgres = PostgreSQLDatabase()
order_service = OrderService(postgres)

order_service.save_order("Order 101")
```

Notice that `OrderService` does not need modification when we switch database implementations.

**Key takeaway:** Depend on abstractions rather than specific implementations.

---

## Summary

| Principle | Main idea |
|---|---|
| SRP | One class should have one responsibility. |
| OCP | Extend functionality without unnecessarily modifying existing code. |
| LSP | Child classes should respect their parent's expected behaviour. |
| ISP | Avoid forcing classes to implement unnecessary methods. |
| DIP | Depend on abstractions instead of concrete implementations. |

## Final Conclusion

SOLID principles help us design software that is easier to maintain, test, extend, and modify.

They do not eliminate dependencies or guarantee that changes will never affect other components.

Instead, they help us organize dependencies and responsibilities so that changes are easier to manage.

**The main objective of SOLID is to make software easier to change without causing unnecessary changes elsewhere.**
