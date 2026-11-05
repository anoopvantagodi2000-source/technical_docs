# Python Dictionary (dict) – Cheat Sheet

> A dictionary is a built-in data structure that stores data as key-value pairs. Dictionaries are ordered (insertion order), mutable, and keys must be unique and hashable.

## 1. Creating a Dictionary

```python
# Empty dictionary
student = {}

# Dictionary with values
student = {
    "name": "Anoop",
    "age": 26,
    "language": "Python"
}

# Using dict()
student = dict(name="Anoop", age=26)

# Nested dictionary
students = {
    "student1": {
        "name": "Anoop",
        "age": 26
    },
    "student2": {
        "name": "Rahul",
        "age": 25
    }
}
```

## 2. Accessing Elements

```python
student = {
    "name": "Anoop",
    "age": 26
}

# Access using key
student["name"]  # Anoop

# Access safely using get()
student.get("age")  # 26

# Return default if key doesn't exist
student.get("city", "Not found")

# Check whether key exists
"name" in student  # True
"city" not in student  # True
```

**Important:** `student["city"]` raises `KeyError` if the key doesn't exist. `student.get("city")` returns `None` by default.

## 3. Adding and Updating Elements

```python
student = {"name": "Anoop"}

# Add a new key
student["age"] = 26

# Update existing key
student["age"] = 27

# Update multiple key-value pairs
student.update({
    "city": "Belgaum",
    "language": "Python"
})

# Set value only if key doesn't exist
student.setdefault("country", "India")
```

## 4. Removing Elements

```python
student = {
    "name": "Anoop",
    "age": 26,
    "city": "Belgaum"
}

# Remove a key
student.pop("age")

# Remove and return the last inserted pair
student.popitem()

# Delete a specific key
del student["name"]

# Remove all elements
student.clear()
```

## 5. Dictionary Methods: keys(), values(), items()

```python
student = {
    "name": "Anoop",
    "age": 26,
    "city": "Belgaum"
}

# Get all keys
student.keys()

# Get all values
student.values()

# Get all key-value pairs
student.items()
```

## 6. Iterating Over Dictionaries

```python
student = {
    "name": "Anoop",
    "age": 26,
    "city": "Belgaum"
}

# Iterate over keys
for key in student:
    print(key)

# Iterate over values
for value in student.values():
    print(value)

# Iterate over key-value pairs
for key, value in student.items():
    print(key, value)
```

## 7. Dictionary Comprehension

Create dictionaries using a concise syntax.

```python
numbers = [1, 2, 3, 4, 5]

# Number as key and square as value
squares = {num: num ** 2 for num in numbers}

# Conditional dictionary comprehension
even_squares = {
    num: num ** 2
    for num in numbers
    if num % 2 == 0
}
```

## 8. Merging Dictionaries

```python
dict1 = {"name": "Anoop"}
dict2 = {"age": 26}

# Using update() (modifies dict1)
dict1.update(dict2)

# Using | (creates a new dictionary, Python 3.9+)
merged = dict1 | dict2

# Using unpacking
merged = {**dict1, **dict2}
```

**Note:** If both dictionaries contain the same key, the value from the right-hand dictionary takes precedence.

## 9. Nested Dictionary

```python
students = {
    "101": {"name": "Anoop", "marks": 85},
    "102": {"name": "Rahul", "marks": 90}
}

# Access nested values
students["101"]["name"]

# Iterate nested dictionary
for student_id, details in students.items():
    print(student_id)
    print(details["name"])
    print(details["marks"])
```

## 10. Common Problem-Solving Patterns

### Frequency Counter

Count how many times each element appears.

```python
numbers = [1, 2, 2, 3, 3, 3]

frequency = {}

for num in numbers:
    frequency[num] = frequency.get(num, 0) + 1

# Output: {1: 1, 2: 2, 3: 3}
```

### Using defaultdict

Avoid manually checking whether a key exists.

```python
from collections import defaultdict

frequency = defaultdict(int)

numbers = [1, 2, 2, 3, 3, 3]

for num in numbers:
    frequency[num] += 1
```

### Grouping Elements

```python
students = [
    ("Anoop", "Java"),
    ("Rahul", "Python"),
    ("Sam", "Java")
]

groups = {}

for name, language in students:
    groups.setdefault(language, []).append(name)

# {'Java': ['Anoop', 'Sam'], 'Python': ['Rahul']}
```

## 11. Useful Dictionary Functions
``` python
data = {"a": 10, "b": 20, "c": 30}

len(data)             # 3
"a" in data           # True
"z" not in data       # True
list(data.keys())     # ['a', 'b', 'c']
list(data.values())   # [10, 20, 30]
list(data.items())    # [('a', 10), ('b', 20), ('c', 30)]
```



