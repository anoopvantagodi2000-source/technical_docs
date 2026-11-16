# Python String Cheat Sheet

## 1. String Creation
```python
s = "Hello"
s = 'Hello'
s = """Multiline String"""
```

## 2. String Indexing
```python
s[0]       # First character
s[-1]      # Last character
s[1]       # Second character
s[-2]      # Second last character
```

## 3. String Slicing
```python
s[start:end]
s[start:end:step]

s[0:3]     # First three characters
s[:3]      # From beginning
s[2:]      # From index 2
s[:]       # Entire string
s[::-1]    # Reverse string
s[::2]     # Every second character
s[-3:]     # Last three characters
```

## 4. String Length
```python
len(s)
```

## 5. Case Conversion

| Method | Description |
|---|---|
| `upper()` | Convert to uppercase |
| `lower()` | Convert to lowercase |
| `capitalize()` | Capitalize first character |
| `title()` | Capitalize each word |
| `swapcase()` | Swap character cases |
| `casefold()` | Convert to lowercase for caseless comparison |

## 6. Searching

| Method | Description |
|---|---|
| `find(x)` | Find first occurrence, returns -1 if absent |
| `rfind(x)` | Find last occurrence |
| `index(x)` | Find first occurrence, raises ValueError if absent |
| `rindex(x)` | Find last occurrence |
| `count(x)` | Count occurrences |
| `startswith(x)` | Check starting characters |
| `endswith(x)` | Check ending characters |

## 7. Checking String Content

| Method | Description |
|---|---|
| `isalpha()` | Check alphabetic characters |
| `isdigit()` | Check digit characters |
| `isnumeric()` | Check numeric characters |
| `isalnum()` | Check letters and numbers |
| `isspace()` | Check whitespace |
| `islower()` | Check lowercase |
| `isupper()` | Check uppercase |
| `istitle()` | Check title case |
| `isascii()` | Check ASCII characters |

## 8. Removing Whitespace and Characters

| Method | Description |
|---|---|
| `strip()` | Remove whitespace from both ends |
| `lstrip()` | Remove whitespace from left |
| `rstrip()` | Remove whitespace from right |
| `removeprefix(x)` | Remove specified prefix |
| `removesuffix(x)` | Remove specified suffix |

## 9. Replacing Characters
```python
s.replace(old, new)
s.replace(old, new, count)
```

## 10. Splitting Strings

| Method | Description |
|---|---|
| `split()` | Split from left |
| `rsplit()` | Split from right |
| `splitlines()` | Split at line boundaries |
| `partition(x)` | Split into three parts at first occurrence |
| `rpartition(x)` | Split into three parts at last occurrence |

## 11. Joining Strings
```python
separator.join(iterable)
```

## 12. String Formatting
```python
name = "Anoop"
age = 26

f"Name: {name}"
f"Age: {age}"

"Name: {}".format(name)
"%s" % name
```

## 13. Character Conversion
```python
ord('A')       # Character to Unicode code point
chr(65)        # Unicode code point to character
```

## 14. Built-in Functions

| Function | Description |
|---|---|
| `len(s)` | String length |
| `min(s)` | Minimum character |
| `max(s)` | Maximum character |
| `sorted(s)` | Return sorted characters as a list |
| `reversed(s)` | Return reverse iterator |
| `enumerate(s)` | Return index and character pairs |
| `list(s)` | Convert to list |
| `str(x)` | Convert to string |

## 15. String Operators

| Operator | Description |
|---|---|
| `+` | Concatenation |
| `*` | Repetition |
| `in` | Check substring membership |
| `not in` | Check absence |
| `==` | Equality |
| `!=` | Inequality |
| `<`, `>` | Lexicographical comparison |

## 16. Important Concepts

- Strings are immutable.
- String indexing starts from `0`.
- Negative indexing starts from `-1`.
- Slicing excludes the ending index.
- Strings support Unicode.
- String methods return new strings instead of modifying the original.


