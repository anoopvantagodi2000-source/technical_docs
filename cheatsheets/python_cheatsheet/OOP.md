# Python OOP Cheat Sheet

## 1. Class and Object
```python
class Student:
    def __init__(self, name):
        self.name = name

    def display(self):
        return self.name

student = Student("Anoop")
```

## 2. Constructor
```python
def __init__(self):
    pass
```
- Initializes object attributes.
- Called automatically when an object is created.

## 3. Instance Variables and Methods
```python
self.name = name

def display(self):
    pass
```
- Instance variables belong to individual objects.
- Instance methods operate on object data.
- `self` refers to the current instance.

## 4. Class Variables and Methods
```python
class Student:
    school = "ABC"

    @classmethod
    def change_school(cls, name):
        cls.school = name
```
- Class variables belong to the class.
- Class methods receive `cls` as the first argument.

## 5. Static Methods
```python
class Calculator:
    @staticmethod
    def add(a, b):
        return a + b
```
- Does not receive `self` or `cls` automatically.
- Used for utility methods.

## 6. Encapsulation
- Bundles data and methods together.
- Controls access to internal attributes.

| Syntax | Meaning |
|---|---|
| `name` | Public |
| `_name` | Protected convention |
| `__name` | Name mangling |

## 7. Inheritance
```python
class Animal:
    pass

class Dog(Animal):
    pass
```
- Allows a class to inherit properties and methods from another class.

**Types of inheritance:**
- Single inheritance
- Multiple inheritance
- Multilevel inheritance
- Hierarchical inheritance
- Hybrid inheritance

## 8. Method Overriding
```python
class Animal:
    def sound(self):
        pass

class Dog(Animal):
    def sound(self):
        print("Bark")
```
- Child class provides its own implementation of a parent method.

## 9. Polymorphism
- Same interface with different implementations.
- Allows different object types to be used through a common interface.

## 10. Abstraction
```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass
```
- Hides implementation details.
- Exposes essential functionality.
- Abstract classes cannot be instantiated while abstract methods remain unimplemented.

## 11. super()
```python
super().__init__()
```
- Accesses the next implementation in the Method Resolution Order (MRO).
- Commonly used to call parent initialization logic.

## 12. Special Methods (Dunder Methods)

| Method | Purpose |
|---|---|
| `__init__()` | Initialize object |
| `__str__()` | User-friendly string representation |
| `__repr__()` | Developer-oriented representation |
| `__len__()` | Defines behavior of `len()` |
| `__eq__()` | Equality comparison |
| `__lt__()` | Less-than comparison |
| `__add__()` | Addition |
| `__del__()` | Object finalization |

## 13. Property Decorator
```python
class Student:
    def __init__(self, age):
        self._age = age

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, value):
        self._age = value
```
- Provides controlled attribute access.
- Allows methods to be accessed like attributes.
- Supports validation through setters.

## 14. Composition
- Represents a HAS-A relationship.
- One class contains an instance of another class.

## 15. Aggregation
- Represents a HAS-A relationship.
- Contained objects can exist independently of the container.

## 16. Method Resolution Order (MRO)
```python
ClassName.mro()
```
- Determines the order in which Python searches for methods and attributes.
- Important in multiple inheritance.

## 17. Four Pillars of OOP

| Pillar | Purpose |
|---|---|
| Encapsulation | Bundle data and methods |
| Inheritance | Reuse existing functionality |
| Polymorphism | Common interface, different behavior |
| Abstraction | Hide implementation details |

## 18. SOLID Principles

| Principle | Meaning |
|---|---|
| SRP | Single Responsibility Principle |
| OCP | Open/Closed Principle |
| LSP | Liskov Substitution Principle |
| ISP | Interface Segregation Principle |
| DIP | Dependency Inversion Principle |

## 19. Useful Built-in Functions

| Function | Purpose |
|---|---|
| `type()` | Returns object's type |
| `isinstance()` | Checks object type |
| `issubclass()` | Checks class inheritance |
| `super()` | Accesses next implementation in MRO |
| `getattr()` | Gets an attribute |
| `setattr()` | Sets an attribute |
| `hasattr()` | Checks attribute existence |
| `delattr()` | Deletes an attribute |

## Important Notes

- Everything in Python is an object.
- `self` represents the current instance.
- `cls` represents the class.
- Python supports multiple inheritance.
- Python uses MRO to resolve inheritance.
- Python does not enforce strict private access.
- `super()` follows MRO, not necessarily just the direct parent.

