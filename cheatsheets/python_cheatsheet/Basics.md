# Getting Started 

### Start the shell in Interactive mode
>$ python

### Quit Interactive mode
> \>>> exit()


### To Run a Script 
>$ python main.py

### To Run a Script in Interactive mode
>$ python -i main.py


## Comments
- always use space after #
- use comments to explain why of your code
```python
# this line will be ignored by interpreter
# print("hello,world") this line be ignored
print("Hello,world")

```

## Data Types
- Python is dynamically typed language
- use None to represent missing value
- use type to check data type

```python
type(2)   # <class 'int'>
type("anoop") # <class 'str'>
type(2.3) # <class 'float'>
type(True) # <class 'bool'>
type(None) # <class 'NoneType'>
```

## Variable
- variable are container which store the value.
- use snake case for naming.
- assignment operator (=) operator is used assign the value to variable.

```python
name = "anoop"
print("hello ", world)

daily_temperature = "23 degree Celsius"
```

## Operators
- \+  "addition operator"
- \* "multiplication operator"
- \\ "division operator"
- \% "modulo operator"

```python

3 + 2 
3 * 2
3 / 2   # division always returns float value
3 // 2  # integer division 
3 % 2    #  returns reminder 
3 ** 2   # 3^2
 
```

## Conditional 
- Python uses indentation for code blocks.
- use 4 spaces per indentation level.
```python
if age < 13:
    print("child")
elif age < 18:
    print("teenager")
else:
  print("adult")
```

## Conditional operator
- evaluates expression either True or False
```python
3 > 2     # greater than
2 < 3     # less than
a >= b    # greater or equal to
a <= b    # less than or equal
a == b    # equal operator
a != b    # not equal to

```
# Python For Loop – Cheat Sheet

## 1. Basic Syntax

A `for` loop is used to iterate over a sequence such as a list, tuple, string, dictionary, or range.

```python
for variable in iterable:
    # statements
```

Example:

```python
numbers = [10, 20, 30]

for number in numbers:
    print(number)
```

Output:
```text
10
20
30
```

## 2. Iterating Over Different Data Types

### List
```python
numbers = [1, 2, 3]

for num in numbers:
    print(num)
```

### Tuple
```python
coordinates = (10, 20, 30)

for coordinate in coordinates:
    print(coordinate)
```

### String
```python
name = "Python"

for char in name:
    print(char)
```

### Dictionary

Iterate over keys:
```python
student = {"name": "Anoop", "age": 26}

for key in student:
    print(key)
```

Iterate over values:
```python
for value in student.values():
    print(value)
```

Iterate over key-value pairs:
```python
for key, value in student.items():
    print(key, value)
```

## 3. Using range()

`range()` generates a sequence of numbers.

### range(stop)
```python
for i in range(5):
    print(i)
```
Output: `0 1 2 3 4`

### range(start, stop)
```python
for i in range(1, 6):
    print(i)
```
Output: `1 2 3 4 5`

### range(start, stop, step)
```python
for i in range(0, 10, 2):
    print(i)
```
Output: `0 2 4 6 8`

### Reverse iteration
```python
for i in range(5, 0, -1):
    print(i)
```
Output: `5 4 3 2 1`

**Important:** The stop value is always excluded.

## 4. Using enumerate()

Use `enumerate()` when you need both the index and the value.

```python
languages = ["Python", "Java", "Go"]

for index, language in enumerate(languages):
    print(index, language)
```

Start indexing from 1:

```python
for index, language in enumerate(languages, start=1):
    print(index, language)
```

## 5. Using zip()

Use `zip()` to iterate over multiple iterables simultaneously.

```python
names = ["Anoop", "Rahul", "Sam"]
ages = [26, 25, 24]

for name, age in zip(names, ages):
    print(name, age)
```

**Note:** `zip()` stops when the shortest iterable is exhausted.

## 6. Nested For Loops

A loop inside another loop.

```python
for i in range(3):
    for j in range(2):
        print(i, j)
```

Useful for:
- Matrix traversal
- 2D lists
- Pattern printing
- Pair comparisons

## 7. Loop Control Statements

### break
Terminates the loop immediately.

```python
for i in range(10):
    if i == 5:
        break
    print(i)
```

### continue
Skips the current iteration and moves to the next one.

```python
for i in range(5):
    if i == 2:
        continue
    print(i)
```

### pass
Does nothing. Used as a placeholder.

```python
for i in range(5):
    pass
```

## 8. List Comprehension

A concise way to create a list using a loop.

Traditional approach:
```python
squares = []

for num in range(5):
    squares.append(num ** 2)
```

Using list comprehension:
```python
squares = [num ** 2 for num in range(5)]
```

With a condition:
```python
even_numbers = [num for num in range(10) if num % 2 == 0]
```

## 9. else With For Loop

The `else` block executes when the loop completes normally without encountering `break`.

```python
for num in range(5):
    print(num)
else:
    print("Loop completed")
```




