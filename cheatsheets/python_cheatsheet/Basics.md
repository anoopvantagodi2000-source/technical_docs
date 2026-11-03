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