# Python: Important Concepts Quick Recall

## 1. Python Basics

- Python is readable, dynamically typed, and indentation creates code blocks.
- A variable is a name that points to an object. You do not declare its type first.
- Everything in Python is an object.
- Use four spaces for indentation.

```python
name = "Ada"
age = 30
is_active = True
```

### Comments
```python
# This is a single-line comment
```

## 2. Input and Output

### Input
`input()` always returns text (`str`). Convert it when a number is needed.

```python
name = input("Enter your name: ")
age = int(input("Enter your age: "))
price = float(input("Enter price: "))
```

### Print
```python
print("Hello")
print("Name:", name)
print(f"Hello, {name}")
```

Useful `print()` options:
```python
print("A", "B", sep="-")   # A-B
print("Loading", end="...") # stays on the same line
```

### Decimal precision
Use an f-string or `format()`.

```python
amount = 12.34567
print(f"{amount:.2f}")        # 12.35
print(format(amount, ".3f"))  # 12.346
```

- `.2f` means show exactly two digits after the decimal point.
- `round(amount, 2)` rounds the number; it does not always display trailing zeroes.

## 3. Built-in Types

### `int` - whole numbers
```python
age = 25
remainder = 10 % 3
whole_part = 10 // 3
```
- Operators: `+`, `-`, `*`, `/`, `//`, `%`, `**`.
- `//` is whole-number division; `%` is remainder.
- Useful: `abs(value)`, `round(value)`, `int("25")`.

### `float` - decimal numbers
```python
price = 19.99
rounded = round(price, 1)
```
- Use for decimal values and measurements.
- Useful: `round(value, digits)`, `abs(value)`.
- Avoid comparing calculated floats with `==`; use `math.isclose()` if needed.

### `str` - text
```python
name = "  Ada  "
clean_name = name.strip().upper()
```
- Strings are immutable: a method returns a new string.
- Important: `.strip()`, `.split()`, `.join()`, `.replace()`, `.lower()`, `.upper()`.
- Use f-strings: `f"Hello, {name}"`.

### Is there a `char` type?
Python has **no separate `char` type**. A single character is a string of length one.

```python
letter = "A"
first_letter = "Python"[0]  # P
```

### `bool` - true or false
```python
is_logged_in = True
has_items = bool(["book"])
```
- Values are `True` and `False`.
- Use `and`, `or`, and `not` in conditions.
- `0`, `None`, `""`, `[]`, `{}`, and `set()` are false-like.

### `list` - ordered and changeable collection
```python
fruits = ["apple", "banana"]
fruits.append("mango")
first = fruits[0]
```
- Allows duplicates and supports indexing.
- Important: `.append()`, `.extend()`, `.pop()`, `.remove()`, `.sort()`.

### `tuple` - ordered and fixed collection
```python
point = (10, 20)
x, y = point
```
- Cannot be changed after creation.
- Use for fixed values such as coordinates.
- Important: `.count()`, `.index()`.

### `set` - unique values
```python
skills = {"python", "sql", "python"}
skills.add("azure")
```
- Removes duplicates and has no fixed order.
- Important: `.add()`, `.update()`, `.remove()`, `.discard()`.
- Good for membership checks: `"python" in skills`.

### `dict` - key-value pairs
```python
user = {"name": "Ada", "age": 30}
user["city"] = "London"
name = user.get("name")
```
- Keys are unique; values can repeat.
- Important: `.get()`, `.keys()`, `.values()`, `.items()`, `.update()`, `.pop()`.

### `None` - no value
```python
result = None
if result is None:
    print("No result")
```
- Use `is None`, not `== None`.

## 4. Operators and Conditions

```python
if age >= 18:
    print("Adult")
elif age >= 13:
    print("Teen")
else:
    print("Child")
```

- Comparison: `==`, `!=`, `<`, `>`, `<=`, `>=`.
- Logical: `and`, `or`, `not`.
- Membership: `in`, `not in`.
- Identity: `is`, `is not`.
- Use `==` to compare values and `is` to check the same object, mainly `None`.

## 5. Loops

```python
for fruit in fruits:
    print(fruit)

for index, fruit in enumerate(fruits):
    print(index, fruit)

for number in range(1, 6):
    print(number)

while count > 0:
    count -= 1
```

- `range(start, stop, step)` stops before `stop`.
- `break` exits a loop.
- `continue` skips to the next loop iteration.
- Use `enumerate()` when both index and value are needed.

### Comprehensions
```python
squares = [number * number for number in range(1, 6)]
even_numbers = [number for number in numbers if number % 2 == 0]
```
- A short way to create lists, sets, and dictionaries.

## 6. Functions

```python
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

message = greet("Ada")
```

- `def` creates a function; `return` sends a value back.
- Parameters receive input; arguments are values passed to them.
- Default values make an argument optional.
- `*args` collects extra positional arguments; `**kwargs` collects extra named arguments.

```python
def total(*numbers):
    return sum(numbers)
```

### Important function rules
- Do not use mutable defaults such as `items=[]`; use `None` and create the list inside.
- Local variables exist only inside the function.
- Use type hints for clarity: `def add(a: int, b: int) -> int:`.

## 7. Errors and Files

### Errors
```python
try:
    age = int(input("Age: "))
except ValueError:
    print("Enter a whole number")
finally:
    print("Done")
```

- Put risky code in `try`.
- Handle expected errors with a specific `except`.
- `finally` always runs.
- Use `raise ValueError("message")` to create an error.

### Files
```python
with open("notes.txt", "r", encoding="utf-8") as file:
    content = file.read()
```

- `with` closes the file automatically.
- Modes: `r` read, `w` write/overwrite, `a` append.

## 8. Modules and Packages

```python
import math
from pathlib import Path

print(math.sqrt(16))
```

- A module is a Python file you import.
- A package is a folder of related modules.
- Use `if __name__ == "__main__":` for code that should run only when the file is executed directly.
- Use a virtual environment to keep project packages separate: `python -m venv .venv`.

## 9. OOP Basics

### Class and object
```python
class Car:
    def __init__(self, brand, year):
        self.brand = brand
        self.year = year

    def describe(self):
        return f"{self.brand} ({self.year})"

car = Car("Ford", 2024)
print(car.describe())
```

- A class is a blueprint; an object is one instance of that class.
- `self` means the current object.
- `__init__` sets up object data.
- Attributes store data; methods define behavior.

### Inheritance and composition
```python
class ElectricCar(Car):
    def charge(self):
        return "Charging"
```

- Inheritance means one class reuses another class's behavior.
- Use `super()` to call a parent class method.
- Prefer composition when one object simply uses another object.

### Useful OOP tools
- `@property`: read data like an attribute while running validation code.
- `@classmethod`: works with the class; often used as an alternate constructor.
- `@staticmethod`: a helper function placed inside a class.
- `@dataclass`: a short way to create classes mainly used to store data.

## 10. Useful Python Concepts

### Mutable vs immutable
- Mutable: `list`, `dict`, `set`. They can change in place.
- Immutable: `int`, `float`, `str`, `tuple`, `bool`. A change creates a new object.

```python
items = [1, 2]
also_items = items
also_items.append(3)
print(items)  # [1, 2, 3]
```

### Copying
- `new_list = old_list.copy()` makes a shallow copy.
- For nested objects, use `copy.deepcopy()` when a fully separate copy is needed.

### Iterators and generators
```python
def count_up_to(limit):
    for number in range(1, limit + 1):
        yield number
```

- A generator produces values one at a time and saves memory.
- `yield` turns a function into a generator.

### Context managers
- `with` is a context manager pattern that guarantees cleanup.
- Common example: files with `with open(...)`.

### Sorting
```python
names = ["Zoe", "Ada", "Ben"]
sorted_names = sorted(names)
names.sort()
```

- `sorted()` returns a new list.
- `.sort()` changes the existing list.

### Concurrency
- `threading`: usually for I/O work.
- `multiprocessing`: usually for CPU-heavy work.
- `asyncio`: many I/O tasks using `async` and `await`.

## 11. Python Compared with Java and C#

| Java/C# feature | Python instead |
|---|---|
| Static type declaration | Dynamic types; add type hints and use Pyright or mypy for checking |
| `char` type | A one-character `str`, for example `"A"` |
| Method overloading by parameter types | Default arguments, `*args`, or separate method names |
| Interfaces | `typing.Protocol` or abstract base classes (`abc.ABC`) |
| `public`, `private`, `protected` keywords | Naming conventions: `_name` means internal; use `@property` for controlled access |
| `null` | `None`; check with `is None` |
| Arrays | `list` for most uses; `tuple` for fixed data; `array` module for typed numeric arrays |
| `switch` | `match` / `case` in Python 3.10+, or `if` / `elif` |
| Checked exceptions | No checked exceptions; document and handle expected exceptions with `try` / `except` |
| Manual memory cleanup | Automatic garbage collection; close files and connections with `with` |
| `Main` method | `if __name__ == "__main__":` block |
| Getters/setters | Direct attributes, then `@property` when logic or validation is needed |

## 12. Must Remember

- `input()` returns a string.
- Use f-strings for readable output and `f"{value:.2f}"` for two decimal places.
- Python does not have `char`; use a one-character string.
- Use `list` for ordered changeable data, `tuple` for fixed data, `set` for unique values, and `dict` for labeled values.
- Use `is None` for no-value checks.
- Use `with` for files and resources.
- Keep functions small and use classes when data and behavior belong together.

## Quick-Reference Summary Table

| # | Concept | Core technique |
|---|---|---|
| 1 | Read input | `input()`; convert with `int()` or `float()` |
| 2 | Print output | `print()` and f-strings: `f"Hello, {name}"` |
| 3 | Decimal precision | `f"{amount:.2f}"` or `format(amount, ".2f")` |
| 4 | Conditions | `if` / `elif` / `else` with `and`, `or`, `not` |
| 5 | Repeat work | `for`, `while`, `range()`, `enumerate()` |
| 6 | Store ordered data | `list`; use `.append()`, `.pop()`, `.sort()` |
| 7 | Store fixed data | `tuple`; unpack with `x, y = point` |
| 8 | Remove duplicates | `set(values)`; test membership with `value in values` |
| 9 | Store labeled data | `dict`; safely read with `.get(key)` |
| 10 | Transform a collection | List comprehension: `[x * 2 for x in values]` |
| 11 | Sort data | `sorted(values)` for new data; `.sort()` to change a list |
| 12 | Write reusable code | `def`, parameters, `return`, default arguments |
| 13 | Handle errors | `try` / `except` / `finally` |
| 14 | Read or write files | `with open(path, mode, encoding="utf-8")` |
| 15 | Work with text | `.strip()`, `.split()`, `.join()`, `.replace()` |
| 16 | Check no value | `value is None` |
| 17 | Avoid shared nested data | `.copy()` or `copy.deepcopy()` |
| 18 | Save memory for large data | Generator function with `yield` |
| 19 | Create objects | `class`, `__init__`, `self`, methods |
| 20 | Reuse OOP behavior | Inheritance, composition, `super()` |
| 21 | Define a required contract | Abstract class: `ABC`; interface-like type: `Protocol` |
| 22 | Pass dependencies into a class | Constructor injection: `Service(dependency)` |


## 13. OOPS in Python with examples

OOP groups related data and behavior inside classes and objects.

### 1. Class and object
A **class** is a blueprint. An **object** is a real item created from that blueprint.

```python
class Student:
    def __init__(self, name):
        self.name = name

student = Student("Ada")
print(student.name)  # Ada
```

### 2. Encapsulation
**Encapsulation** keeps data and the rules for changing it together. In Python, use a property when a value needs validation.

```python
class BankAccount:
    def __init__(self, balance):
        self._balance = balance

    @property
    def balance(self):
        return self._balance

    def deposit(self, amount):
        if amount > 0:
            self._balance += amount

account = BankAccount(100)
account.deposit(50)
print(account.balance)  # 150
```

- `_balance` means internal-use data by convention.
- The object controls how its balance changes through `deposit()`.

### 3. Inheritance
**Inheritance** lets a child class reuse and extend a parent class.

```python
class Animal:
    def speak(self):
        return "Some sound"

class Dog(Animal):
    def speak(self):
        return "Woof"

dog = Dog()
print(dog.speak())  # Woof
```

- `Dog` inherits from `Animal` and replaces the `speak()` behavior.
- Use `super()` when a child also needs to call the parent implementation.

### 4. Polymorphism
**Polymorphism** means the same method call can work differently for different object types.

```python
class Cat:
    def speak(self):
        return "Meow"

animals = [Dog(), Cat()]
for animal in animals:
    print(animal.speak())
```

- The loop calls `speak()` the same way for both objects.
- Python cares that an object has the required method. This is called duck typing.

### 5. Abstraction
**Abstraction** shows what an object can do while hiding how it does it. Use an abstract class to require child classes to implement a method.

```python
from abc import ABC, abstractmethod

class Payment(ABC):
    @abstractmethod
    def pay(self, amount):
        pass

class CardPayment(Payment):
    def pay(self, amount):
        return f"Paid {amount} by card"

payment = CardPayment()
print(payment.pay(50))
```

- `Payment` describes the required `pay()` method.
- Each payment type can implement `pay()` differently.
- `Payment()` cannot be created directly because `pay()` has no implementation.

### 6. Interface with `Protocol`
Python does not have a special `interface` keyword. Use `typing.Protocol` to describe the methods an object must provide.

```python
from typing import Protocol

class Notifier(Protocol):
    def send(self, message: str) -> None:
        ...

class EmailNotifier:
    def send(self, message: str) -> None:
        print(f"Email: {message}")

def notify_user(notifier: Notifier) -> None:
    notifier.send("Your order is ready")

notify_user(EmailNotifier())
```

- `EmailNotifier` does not need to inherit from `Notifier`.
- It works because it has the required `send()` method. This is structural typing.
- Use an abstract class (`ABC`) when child classes should share common code; use a `Protocol` when only the required behavior matters.

### 7. Dependency injection
**Dependency injection (DI)** means giving an object the dependency it needs instead of creating that dependency inside the object.

```python
class OrderService:
    def __init__(self, notifier: Notifier):
        self.notifier = notifier

    def place_order(self) -> None:
        self.notifier.send("Order placed")

service = OrderService(EmailNotifier())
service.place_order()
```

- `OrderService` depends on the `Notifier` interface, not on `EmailNotifier` directly.
- Another notifier, such as `SmsNotifier`, can be passed in without changing `OrderService`.
- This makes code easier to test: pass a simple fake notifier in tests.

### 8. Composition
**Composition** means one object uses another object. Prefer it when the relationship is "has-a", not "is-a".

```python
class Engine:
    def start(self):
        return "Engine started"

class Vehicle:
    def __init__(self):
        self.engine = Engine()

    def start(self):
        return self.engine.start()

car = Vehicle()
print(car.start())
```

- A vehicle **has an** engine, so composition is a good fit.
