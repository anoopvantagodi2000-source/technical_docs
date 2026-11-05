# Python Arrays (Lists) – Cheat Sheet

> In Python, the built-in `list` is the most commonly used array-like data structure. Lists are ordered, mutable, and allow duplicate elements.

## 1. Creating a List

```python
# Empty list
numbers = []

# List of integers
numbers = [10, 20, 30, 40]

# List of strings
languages = ["Python", "Java", "Go"]

# Mixed data types
data = [10, "Python", True, 3.14]

# Nested list (2D list)
matrix = [
    [1, 2, 3],
    [4, 5, 6]
]
```

## 2. Accessing Elements

```python
numbers = [10, 20, 30, 40, 50]

numbers[0]    # 10 (first element)
numbers[1]    # 20
numbers[-1]   # 50 (last element)
numbers[-2]   # 40 (second last element)
```

## 3. Slicing

Syntax: `list[start:stop:step]`

```python
numbers = [10, 20, 30, 40, 50]

numbers[1:3]   # [20, 30]
numbers[:3]    # [10, 20, 30]
numbers[2:]    # [30, 40, 50]
numbers[:]     # Copy of the list
numbers[::2]   # [10, 30, 50]
numbers[::-1]  # [50, 40, 30, 20, 10]
```

## 4. Adding Elements

```python
numbers = [10, 20, 30]

# Add element at the end
numbers.append(40)

# Insert at a specific index
numbers.insert(1, 15)

# Add multiple elements
numbers.extend([50, 60])

# Concatenate lists
result = numbers + [70, 80]
```

## 5. Removing Elements

```python
numbers = [10, 20, 30, 40, 50]

# Remove by value (first occurrence)
numbers.remove(20)

# Remove by index
numbers.pop(0)

# Remove last element
numbers.pop()

# Delete using index
del numbers[1]

# Clear all elements
numbers.clear()
```

## 6. Searching Elements

```python
numbers = [10, 20, 30, 20, 40]

# Check whether element exists
20 in numbers       # True

# Check whether element doesn't exist
50 not in numbers   # True

# Find index of first occurrence
numbers.index(20)   # 1

# Count occurrences
numbers.count(20)   # 2
```

## 7. Sorting and Reversing

```python
numbers = [40, 10, 30, 20]

# Sort ascending (modifies original list)
numbers.sort()

# Sort descending
numbers.sort(reverse=True)

# Return a new sorted list
sorted_numbers = sorted(numbers)

# Reverse original list
numbers.reverse()
```

## 8. Useful Built-in Functions

```python
numbers = [10, 20, 30, 40]

len(numbers)    # 4
min(numbers)    # 10
max(numbers)    # 40
sum(numbers)    # 100

```

## 9. Iterating Over Lists

```python
numbers = [10, 20, 30]

# Direct iteration
for num in numbers:
    print(num)

# Using index
for i in range(len(numbers)):
    print(numbers[i])

# Using enumerate
for index, value in enumerate(numbers):
    print(index, value)
```

## 10. List Comprehension

```python
numbers = [1, 2, 3, 4, 5]

# Create squares
squares = [num ** 2 for num in numbers]

# Filter even numbers
evens = [num for num in numbers if num % 2 == 0]

# Conditional transformation
result = ["Even" if num % 2 == 0 else "Odd" for num in numbers]
```

## 11. Copying Lists

```python
numbers = [10, 20, 30]

# Reference assignment (not a copy)
a = numbers

# Shallow copies
b = numbers.copy()
c = numbers[:]
d = list(numbers)
```

**Important:** `a = numbers` does not create a new list. Both variables reference the same list.

## 12. Nested Lists (2D Arrays)

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

# Access an element
matrix[0][1]  # 2

# Iterate through matrix
for row in matrix:
    for value in row:
        print(value)
```
