# Python Cheat Sheet

A quick revision guide for common Python concepts.

---

## 1. Array Methods

In Python, the most common array-like structure is a `list`.

```python
numbers = [10, 20, 30, 40]
```

### Accessing elements

```python
numbers[0]       # first item
numbers[-1]      # last item
numbers[1:3]     # slicing
```

### Common list methods

| Method | Use |
|---|---|
| `append(x)` | Add one item at the end |
| `extend(items)` | Add multiple items |
| `insert(i, x)` | Add an item at a position |
| `remove(x)` | Remove the first matching item |
| `pop()` | Remove and return an item |
| `clear()` | Remove all items |
| `index(x)` | Find the position of an item |
| `count(x)` | Count occurrences |
| `sort()` | Sort the original list |
| `reverse()` | Reverse the original list |
| `copy()` | Make a shallow copy |

Useful built-ins:

```python
len(numbers)
max(numbers)
min(numbers)
sum(numbers)
sorted(numbers)
```

### `sort()` vs `sorted()`

```python
numbers.sort()          # changes the original list
new_list = sorted(numbers)  # returns a new sorted list
```

---

## 2. String Methods

A string is a sequence of characters.

```python
text = "Hello Python"
```

### Accessing characters

```python
text[0]
text[-1]
text[0:5]
```

### Common string methods

| Method | Use |
|---|---|
| `lower()` | Convert to lowercase |
| `upper()` | Convert to uppercase |
| `capitalize()` | Capitalize first character |
| `title()` | Capitalize each word |
| `strip()` | Remove spaces from both ends |
| `lstrip()` | Remove spaces from the left |
| `rstrip()` | Remove spaces from the right |
| `replace(a, b)` | Replace text |
| `split()` | Split a string into a list |
| `join()` | Join items into a string |
| `find()` | Find the position of text |
| `count()` | Count occurrences |
| `startswith()` | Check starting text |
| `endswith()` | Check ending text |
| `isdigit()` | Check whether characters are digits |
| `isalpha()` | Check whether characters are letters |
| `isalnum()` | Check letters and numbers |

Example:

```python
text = "hello world"

text.upper()                  # HELLO WORLD
text.replace("world", "Python")
text.split()                  # ['hello', 'world']
"-".join(["hello", "world"])  # hello-world
```

### Important

Strings are immutable. String methods return a new string.

```python
text = "hello"
text.upper()

# text is still "hello"
```

To save the change:

```python
text = text.upper()
```

---

## 3. Objects and Object-Oriented Programming

### Class

A class is a blueprint for creating objects.

```python
class Student:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def introduce(self):
        print(f"My name is {self.name}")
```

### Object

An object is an instance of a class.

```python
student1 = Student("Adi", 23)
student1.introduce()
```

### Important terms

| Term | Meaning |
|---|---|
| Class | Blueprint for objects |
| Object | Instance of a class |
| Attribute | Data stored in an object |
| Method | Function inside a class |
| `__init__()` | Constructor used to initialize an object |
| `self` | Refers to the current object |

### Four main OOP concepts

**Encapsulation** - Keep data and the methods that work on it together.

```python
class BankAccount:
    def __init__(self, balance):
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount
```

**Inheritance** - A child class can reuse a parent class.

```python
class Animal:
    def speak(self):
        print("Animal sound")

class Dog(Animal):
    pass

dog = Dog()
dog.speak()
```

**Polymorphism** - The same method name can behave differently for different objects.

```python
class Dog:
    def sound(self):
        print("Bark")

class Cat:
    def sound(self):
        print("Meow")

for animal in [Dog(), Cat()]:
    animal.sound()
```

**Abstraction** - Show important details and hide unnecessary implementation details.

```python
from abc import ABC, abstractmethod

class Animal(ABC):
    @abstractmethod
    def sound(self):
        pass
```

### Quick memory trick

```text
Encapsulation  -> keep together
Inheritance    -> reuse
Polymorphism   -> many forms
Abstraction    -> hide details
```

---

## 4. Decorators

A decorator adds or changes the behavior of a function without changing its original code.

```python
def decorator(func):
    def wrapper():
        print("Before function")
        func()
        print("After function")
    return wrapper

@decorator
def greet():
    print("Hello")

greet()
```

Output:

```text
Before function
Hello
After function
```

### What does `@decorator` mean?

This:

```python
@decorator
def greet():
    pass
```

is essentially:

```python
def greet():
    pass

greet = decorator(greet)
```

### Decorators with arguments

Use `*args` and `**kwargs` when the original function can receive arguments.

```python
def decorator(func):
    def wrapper(*args, **kwargs):
        print("Function started")
        result = func(*args, **kwargs)
        print("Function finished")
        return result
    return wrapper
```

### Common uses

- Logging
- Authentication
- Authorization
- Validation
- Measuring execution time
- Caching

Common built-in decorators include:

```python
@staticmethod
@classmethod
@property
```

---

## 5. virtualenv

A virtual environment creates an isolated Python environment for a project. It prevents packages and versions from different projects from conflicting.

### Create

```bash
python3 -m venv venv
```

### Activate on Linux/macOS

```bash
source venv/bin/activate
```

### Activate on Windows

```powershell
venv\Scripts\activate
```

### Leave the environment

```bash
deactivate
```

After activation, the terminal normally shows:

```text
(venv)
```

### Why use it?

For example:

```text
Project A -> Django 4.x
Project B -> Django 5.x
```

Separate virtual environments allow both projects to use their required versions.

---

## 6. pip Package Manager

`pip` is used to install and manage Python packages.

### Install

```bash
pip install requests
```

### Install a specific version

```bash
pip install requests==2.32.3
```

### Upgrade

```bash
pip install --upgrade requests
```

### Uninstall

```bash
pip uninstall requests
```

### Show package information

```bash
pip show requests
```

### List packages

```bash
pip list
```

### Save dependencies

```bash
pip freeze > requirements.txt
```

### Install dependencies from a file

```bash
pip install -r requirements.txt
```

### Common project setup

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

---

## 7. PEP 8 Standards Summary

PEP 8 is the main Python style guide. It helps make code readable and consistent.

### Indentation

Use 4 spaces.

```python
if age >= 18:
    print("Adult")
```

### Variable and function names

Use `snake_case`.

```python
student_name = "Adi"

def calculate_total():
    pass
```

### Class names

Use `PascalCase`.

```python
class StudentDetails:
    pass
```

### Constants

Use uppercase with underscores.

```python
MAX_USERS = 100
PI_VALUE = 3.14
```

### Spaces around operators

```python
total = price + tax
```

Avoid:

```python
total=price+tax
```

### Space after commas

```python
numbers = [10, 20, 30]
```

### Imports

Keep imports at the top.

```python
import os
import sys

import requests
```

### Blank lines

Use blank lines to separate logical sections.

### Comments

Use comments when they make the code easier to understand.

```python
# Calculate the final price
final_price = price - discount
```

### Line length

PEP 8 traditionally recommends a maximum of 79 characters for code and documentation lines. Projects may use a different formatter configuration.

### Main idea

PEP 8 helps make Python code:

- Readable
- Consistent
- Easy to maintain
- Easy for other developers to understand

---

# Quick Revision

## Lists

```python
items.append(x)
items.extend(values)
items.insert(index, x)
items.remove(x)
items.pop()
items.clear()
items.index(x)
items.count(x)
items.sort()
items.reverse()
```

## Strings

```python
text.lower()
text.upper()
text.strip()
text.replace(old, new)
text.split()
separator.join(items)
text.find(value)
text.count(value)
text.startswith(value)
text.endswith(value)
```

## OOP

```text
Class      -> blueprint
Object     -> instance
Attribute  -> data
Method     -> behavior
self       -> current object
```

```text
Encapsulation -> keep together
Inheritance   -> reuse
Polymorphism  -> different behavior
Abstraction   -> hide details
```

## Decorators

```python
@decorator
def function():
    pass
```

Think:

```text
Function -> Decorator -> Modified behavior
```

## virtualenv

```bash
python3 -m venv venv
source venv/bin/activate
deactivate
```

## pip

```bash
pip install package
pip uninstall package
pip list
pip show package
pip freeze > requirements.txt
pip install -r requirements.txt
```

## PEP 8

```text
4 spaces
snake_case       -> variables/functions
PascalCase       -> classes
UPPER_CASE       -> constants
spaces around operators
space after commas
imports at the top
readable and consistent code
```
