# Python Decorators – Technical Documentation

## 1. What is a Decorator?

A decorator is a function that takes another function as an argument, extends or modifies its behavior, and returns a new function without modifying the original function's source code.

**Common use cases:**
- Logging
- Authentication and authorization
- Execution time measurement
- Input validation
- Caching
- Access control

## 2. Concepts Behind Decorators

Decorators rely on three important Python concepts:

- **First-class functions:** Functions can be assigned to variables, passed as arguments, and returned from other functions.
- **Higher-order functions:** Functions that accept other functions as arguments or return functions.
- **Closures:** Inner functions that retain access to variables from their enclosing function.

## 3. First-Class Functions

Functions are first-class objects in Python.

They can be:
- Assigned to variables.
- Passed as arguments.
- Returned from other functions.
- Stored in data structures.

```python
def greet():
    print("Hello")

message = greet
message()
```

## 4. Higher-Order Functions

A higher-order function accepts another function as an argument or returns a function.

```python
def greet():
    print("Hello")

def execute(func):
    func()

execute(greet)
```

## 5. Closures

A closure is an inner function that remembers variables from its enclosing scope, even after the enclosing function has finished execution.

```python
def outer():
    message = "Hello"

    def inner():
        print(message)

    return inner

func = outer()
func()
```

Closures are an important building block of decorators.

## 6. Creating a Basic Decorator

A decorator wraps an existing function with additional functionality.

```python
def my_decorator(func):

    def wrapper():
        print("Before function execution")

        func()

        print("After function execution")

    return wrapper


def greet():
    print("Hello")


greet = my_decorator(greet)

greet()
```

**Execution flow:**
1. The original function is passed to the decorator.
2. The decorator receives the function as an argument.
3. The inner wrapper function is created.
4. The wrapper function is returned.
5. The original function name now references the wrapper.
6. Calling the function executes the wrapper.

## 7. Using the @ Syntax

Python provides the `@` syntax to apply decorators.

```python
def my_decorator(func):

    def wrapper():
        print("Before execution")
        func()
        print("After execution")

    return wrapper


@my_decorator
def greet():
    print("Hello")


greet()
```

The following syntax:

```python
@my_decorator
def greet():
    pass
```

Is equivalent to:

```python
def greet():
    pass

greet = my_decorator(greet)
```

## 8. Decorators with Arguments

Use `*args` and `**kwargs` to support functions with different arguments.

```python
def my_decorator(func):

    def wrapper(*args, **kwargs):
        print("Before execution")

        result = func(*args, **kwargs)

        print("After execution")

        return result

    return wrapper


@my_decorator
def add(a, b):
    return a + b


result = add(10, 20)
```

- `*args` captures positional arguments.
- `**kwargs` captures keyword arguments.
- Returning the result preserves the original function's return value.

## 9. Preserving Function Metadata

When a decorator wraps a function, the wrapper can replace the original function's metadata, such as its name and docstring.

Use `functools.wraps` to preserve this metadata.

```python
from functools import wraps

def my_decorator(func):

    @wraps(func)
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)

    return wrapper
```

## 10. Decorators with Parameters

A decorator can accept its own arguments. This requires an additional outer function.

```python
def repeat(times):

    def decorator(func):

        def wrapper(*args, **kwargs):
            for _ in range(times):
                func(*args, **kwargs)

        return wrapper

    return decorator


@repeat(3)
def greet():
    print("Hello")


greet()
```

**Execution flow:**
1. `repeat(3)` is called.
2. It returns the decorator function.
3. The original function is passed to the decorator.
4. The decorator returns the wrapper.
5. Calling the function executes the wrapper three times.

## 11. Multiple Decorators

Python allows multiple decorators to be applied to the same function.

```python
@decorator_one
@decorator_two
def greet():
    print("Hello")
```

Equivalent to:

```python
greet = decorator_one(decorator_two(greet))
```

Decorators are applied from bottom to top.

## 12. Built-in Decorators

| Decorator | Purpose |
|---|---|
| `@staticmethod` | Defines a method without automatic instance or class binding |
| `@classmethod` | Defines a method that receives the class as its first argument |
| `@property` | Exposes a method as an attribute |
| `@functools.wraps` | Preserves wrapped function metadata |
| `@functools.cache` | Caches function results |
| `@functools.lru_cache` | Caches function results with a size limit |

## 13. Class-Based Decorators

A class can also be used as a decorator by implementing the `__call__()` method.

```python
class MyDecorator:

    def __init__(self, func):
        self.func = func

    def __call__(self, *args, **kwargs):
        print("Before execution")

        result = self.func(*args, **kwargs)

        print("After execution")

        return result


@MyDecorator
def greet():
    print("Hello")


greet()
```

An instance of `MyDecorator` becomes callable because it implements `__call__()`.

## 14. Real-World Applications

| Application | Description |
|---|---|
| Logging | Record function execution |
| Authentication | Verify user identity |
| Authorization | Check user permissions |
| Caching | Store previously computed results |
| Validation | Validate function inputs |
| Performance monitoring | Measure execution time |
| Rate limiting | Restrict request frequency |

## 15. Important Concepts

- Decorators extend or modify function behavior.
- Functions are first-class objects in Python.
- Decorators use higher-order functions and closures.
- The `@` syntax is syntactic sugar for function reassignment.
- Use `*args` and `**kwargs` to support different function arguments.
- Use `functools.wraps` to preserve function metadata.
- Decorators can accept their own arguments.
- Multiple decorators can be applied to a function.
- Classes can implement decorators using `__call__()`.



## Key Takeaway

A decorator is a callable that takes another callable and returns a callable, usually adding behavior around the original execution.

Understanding closures and higher-order functions makes decorators much easier to understand.
