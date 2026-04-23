# Python Basics Guide

A practical introduction to Python programming.

---

## Table of Contents

1. [Running Python](#running-python)
2. [Variables and Data Types](#variables-and-data-types)
3. [Strings](#strings)
4. [Numbers](#numbers)
5. [Booleans and None](#booleans-and-none)
6. [Lists](#lists)
7. [Tuples](#tuples)
8. [Dictionaries](#dictionaries)
9. [Sets](#sets)
10. [Conditionals](#conditionals)
11. [Loops](#loops)
12. [Functions](#functions)
13. [Modules and Imports](#modules-and-imports)
14. [File I/O](#file-io)
15. [Error Handling](#error-handling)
16. [Classes](#classes)
17. [List Comprehensions](#list-comprehensions)
18. [Quick Reference Cheat Sheet](#quick-reference-cheat-sheet)

---

## Running Python

```bash
python --version          # Check Python version
python script.py          # Run a script
python3 script.py         # Use Python 3 explicitly (some systems need this)
python                    # Open an interactive REPL (Ctrl+D or exit() to quit)
```

---

## Variables and Data Types

Python is **dynamically typed** — you don't declare types, and variables can be reassigned to any type.

```python
name = "Alice"         # str
age = 30               # int
height = 1.75          # float
is_active = True       # bool
score = None           # NoneType

# Multiple assignment
x = y = z = 0

# Swap values
a, b = 1, 2
a, b = b, a            # a=2, b=1

# Check a type
type(name)             # <class 'str'>
isinstance(age, int)   # True
```

---

## Strings

```python
greeting = "Hello, World"
greeting = 'Single quotes also work'

# Multi-line
message = """
Line one
Line two
"""

# f-strings (recommended for formatting)
name = "Alice"
print(f"Hello, {name}!")                  # Hello, Alice!
print(f"2 + 2 = {2 + 2}")                # 2 + 2 = 4
print(f"{3.14159:.2f}")                   # 3.14 (2 decimal places)

# Common methods
s = "  Hello, World!  "
s.strip()              # "Hello, World!"    (remove whitespace)
s.lower()              # "  hello, world!  "
s.upper()              # "  HELLO, WORLD!  "
s.replace("World", "Python")  # "  Hello, Python!  "
s.split(", ")          # ["  Hello", "World!  "]
", ".join(["a", "b", "c"])    # "a, b, c"
s.startswith("  Hello")       # True
s.endswith("!  ")             # True
"world" in s.lower()          # True

# Indexing and slicing
s = "Hello"
s[0]       # "H"
s[-1]      # "o"
s[1:4]     # "ell"
s[:3]      # "Hel"
s[::2]     # "Hlo"  (every 2nd character)
s[::-1]    # "olleH" (reversed)

len(s)     # 5
```

---

## Numbers

```python
# Integer
x = 10
y = -3

# Float
pi = 3.14
sci = 1.5e6        # 1,500,000.0

# Arithmetic
10 + 3     # 13
10 - 3     # 7
10 * 3     # 30
10 / 3     # 3.3333...  (always returns float)
10 // 3    # 3          (floor division, returns int)
10 % 3     # 1          (remainder / modulo)
2 ** 10    # 1024       (power)

# Useful built-ins
abs(-5)           # 5
round(3.14159, 2) # 3.14
min(3, 1, 4)      # 1
max(3, 1, 4)      # 4
sum([1, 2, 3])    # 6

# Type conversion
int("42")         # 42
float("3.14")     # 3.14
str(100)          # "100"
```

---

## Booleans and None

```python
is_ready = True
is_empty = False

# Truthy and falsy values
# Falsy: False, 0, 0.0, "", [], {}, set(), None
# Everything else is truthy

bool(0)     # False
bool("")    # False
bool([])    # False
bool(None)  # False
bool(1)     # True
bool("hi")  # True

# None — represents the absence of a value
result = None
result is None    # True (use 'is', not '==', to check for None)
```

---

## Lists

Lists are ordered, mutable sequences.

```python
fruits = ["apple", "banana", "cherry"]

# Indexing and slicing
fruits[0]          # "apple"
fruits[-1]         # "cherry"
fruits[0:2]        # ["apple", "banana"]

# Adding elements
fruits.append("mango")           # Add to end
fruits.insert(1, "blueberry")    # Insert at index
fruits.extend(["kiwi", "lime"])  # Add multiple items

# Removing elements
fruits.remove("banana")          # Remove by value (first occurrence)
fruits.pop()                     # Remove and return last item
fruits.pop(0)                    # Remove and return item at index

# Sorting
fruits.sort()                    # Sort in place (ascending)
fruits.sort(reverse=True)        # Sort descending
sorted(fruits)                   # Return a new sorted list

# Other operations
len(fruits)                      # Number of items
"apple" in fruits                # True/False membership test
fruits.count("apple")            # Number of occurrences
fruits.index("apple")            # Index of first occurrence
fruits.reverse()                 # Reverse in place
fruits.copy()                    # Shallow copy

# Looping
for fruit in fruits:
    print(fruit)

for i, fruit in enumerate(fruits):
    print(i, fruit)              # 0 apple, 1 banana, ...
```

---

## Tuples

Tuples are ordered and **immutable** (cannot be changed after creation).

```python
point = (3, 7)
rgb = (255, 128, 0)

x, y = point        # Unpacking
r, g, b = rgb

point[0]            # 3
len(point)          # 2

# Single-item tuple needs a trailing comma
single = (42,)

# Tuples are faster than lists and signal "this shouldn't change"
```

---

## Dictionaries

Dictionaries store key-value pairs. Keys must be unique and immutable.

```python
user = {
    "name": "Alice",
    "age": 30,
    "city": "London"
}

# Access
user["name"]                  # "Alice"
user.get("email")             # None (no KeyError if missing)
user.get("email", "N/A")      # "N/A" (default value)

# Add or update
user["email"] = "alice@example.com"
user.update({"age": 31, "country": "UK"})

# Remove
del user["city"]
user.pop("country")           # Remove and return value

# Check membership
"name" in user                # True

# Looping
for key in user:
    print(key)

for key, value in user.items():
    print(key, value)

user.keys()                   # dict_keys(["name", "age", ...])
user.values()                 # dict_values(["Alice", 31, ...])

# Nested dict
config = {
    "db": {"host": "localhost", "port": 5432},
    "debug": True
}
config["db"]["host"]          # "localhost"
```

---

## Sets

Sets are unordered collections of **unique** values.

```python
tags = {"python", "web", "backend"}
tags.add("api")
tags.remove("web")            # KeyError if not found
tags.discard("web")           # No error if not found

"python" in tags              # True

# Set operations
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

a | b    # Union: {1, 2, 3, 4, 5, 6}
a & b    # Intersection: {3, 4}
a - b    # Difference: {1, 2}
a ^ b    # Symmetric difference: {1, 2, 5, 6}

# Convert list to set to remove duplicates
unique = list(set([1, 2, 2, 3, 3, 3]))  # [1, 2, 3]
```

---

## Conditionals

```python
age = 25

if age < 18:
    print("Minor")
elif age < 65:
    print("Adult")
else:
    print("Senior")
```

### Ternary Expression

```python
label = "Adult" if age >= 18 else "Minor"
```

### Match Statement (Python 3.10+)

```python
status = "shipped"

match status:
    case "pending":
        print("Waiting")
    case "shipped" | "delivered":
        print("On the way")
    case _:
        print("Unknown")
```

---

## Loops

### For Loop

```python
for i in range(5):           # 0, 1, 2, 3, 4
    print(i)

for i in range(2, 10, 2):   # 2, 4, 6, 8
    print(i)

for item in ["a", "b", "c"]:
    print(item)
```

### While Loop

```python
count = 0
while count < 5:
    print(count)
    count += 1
```

### Loop Control

```python
for i in range(10):
    if i == 3:
        continue          # Skip this iteration
    if i == 7:
        break             # Stop the loop entirely
    print(i)
```

### Useful Loop Patterns

```python
# Loop with index
for i, val in enumerate(["a", "b", "c"]):
    print(i, val)         # 0 a, 1 b, 2 c

# Loop two lists together
names = ["Alice", "Bob"]
scores = [95, 87]
for name, score in zip(names, scores):
    print(name, score)    # Alice 95, Bob 87

# Loop a dict
for key, value in my_dict.items():
    print(f"{key}: {value}")
```

---

## Functions

```python
def greet(name):
    return f"Hello, {name}!"

greet("Alice")              # "Hello, Alice!"
```

### Default Parameters

```python
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

greet("Alice")              # "Hello, Alice!"
greet("Alice", "Hi")        # "Hi, Alice!"
```

### Keyword Arguments

```python
greet(greeting="Hey", name="Bob")  # "Hey, Bob!"
```

### Variable-Length Arguments

```python
def add(*numbers):               # Receives a tuple
    return sum(numbers)

add(1, 2, 3, 4)                  # 10

def info(**kwargs):              # Receives a dict
    for key, val in kwargs.items():
        print(f"{key}: {val}")

info(name="Alice", age=30)       # name: Alice / age: 30
```

### Lambda Functions

Anonymous one-liner functions:

```python
square = lambda x: x ** 2
square(5)                        # 25

# Often used with sorted, map, filter
people = [{"name": "Bob", "age": 25}, {"name": "Alice", "age": 30}]
people.sort(key=lambda p: p["age"])
```

---

## Modules and Imports

```python
import math
math.sqrt(16)         # 4.0
math.pi               # 3.14159...

import os
os.getcwd()           # Current directory
os.listdir(".")       # List files

import sys
sys.argv              # Command-line arguments
sys.exit(0)           # Exit with a code

from datetime import datetime, timedelta
now = datetime.now()
tomorrow = now + timedelta(days=1)

import json
import random
random.choice(["a", "b", "c"])
random.randint(1, 10)

# Install third-party packages with pip:
# pip install requests
import requests
response = requests.get("https://api.example.com/data")
data = response.json()
```

---

## File I/O

```python
# Read entire file
with open("file.txt", "r") as f:
    content = f.read()

# Read line by line
with open("file.txt", "r") as f:
    for line in f:
        print(line.strip())

# Read all lines into a list
with open("file.txt", "r") as f:
    lines = f.readlines()

# Write to file (overwrites)
with open("file.txt", "w") as f:
    f.write("Hello\n")
    f.write("World\n")

# Append to file
with open("file.txt", "a") as f:
    f.write("New line\n")
```

> Always use `with open(...)` — it automatically closes the file even if an error occurs.

---

## Error Handling

```python
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero")
except (TypeError, ValueError) as e:
    print(f"Error: {e}")
except Exception as e:
    print(f"Unexpected error: {e}")
else:
    print("Success")          # Runs only if no exception occurred
finally:
    print("Always runs")      # Cleanup code — always executes

# Raise an exception
def set_age(age):
    if age < 0:
        raise ValueError("Age cannot be negative")
    return age
```

---

## Classes

```python
class Dog:
    # Class variable (shared by all instances)
    species = "Canis familiaris"

    # Constructor
    def __init__(self, name, age):
        self.name = name    # Instance variable
        self.age = age

    # Instance method
    def bark(self):
        return f"{self.name} says woof!"

    # String representation
    def __str__(self):
        return f"Dog({self.name}, {self.age})"


rex = Dog("Rex", 3)
rex.bark()              # "Rex says woof!"
print(rex)              # Dog(Rex, 3)
rex.name                # "Rex"
Dog.species             # "Canis familiaris"


# Inheritance
class GoldenRetriever(Dog):
    def fetch(self):
        return f"{self.name} fetches the ball!"

    # Override parent method
    def bark(self):
        return f"{self.name} says woof woof!"


buddy = GoldenRetriever("Buddy", 2)
buddy.bark()            # "Buddy says woof woof!"
buddy.fetch()           # "Buddy fetches the ball!"
isinstance(buddy, Dog)  # True
```

---

## List Comprehensions

A concise way to build lists (and dicts/sets):

```python
# Basic form: [expression for item in iterable]
squares = [x ** 2 for x in range(6)]
# [0, 1, 4, 9, 16, 25]

# With a filter condition
evens = [x for x in range(10) if x % 2 == 0]
# [0, 2, 4, 6, 8]

# Transforming strings
words = ["hello", "world"]
upper = [w.upper() for w in words]
# ["HELLO", "WORLD"]

# Dict comprehension
squares_dict = {x: x**2 for x in range(5)}
# {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# Set comprehension
unique_lengths = {len(w) for w in ["hi", "hello", "hey"]}
# {2, 5, 3}
```

---

## Quick Reference Cheat Sheet

| Task | Syntax |
|------|--------|
| Variables | `x = 5` |
| f-string | `f"Hello {name}"` |
| List | `[1, 2, 3]` |
| Tuple | `(1, 2, 3)` |
| Dict | `{"key": "value"}` |
| Set | `{1, 2, 3}` |
| If/elif/else | `if x > 0: ...` |
| For loop | `for i in range(10): ...` |
| While loop | `while x > 0: ...` |
| Define function | `def name(args): ...` |
| Return value | `return x` |
| Lambda | `lambda x: x * 2` |
| List comp | `[x*2 for x in lst]` |
| Import | `import os` |
| From import | `from math import sqrt` |
| Open file | `with open("f.txt") as f:` |
| Try/except | `try: ... except Error: ...` |
| Class | `class MyClass: ...` |
| Constructor | `def __init__(self, ...): ...` |
| String slice | `s[1:4]` |
| List append | `lst.append(val)` |
| Dict get | `d.get("key", default)` |
| Length | `len(x)` |
| Type check | `isinstance(x, int)` |
| None check | `x is None` |
| Print | `print("Hello")` |

---

*Part of the learning-resources collection.*
