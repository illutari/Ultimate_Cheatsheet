# Python vs Java, C#, Rust - Cheatsheet

**Focus:** Key syntactic, semantic, and idiomatic differences  
**Python Version:** 3.12+  
**Target Languages:** Java, C#, Rust

## 1. Core Philosophy & Syntax

| Feature                  | Python                                      | Java / C#                                   | Rust                                      |
|--------------------------|---------------------------------------------|---------------------------------------------|-------------------------------------------|
| **Code Blocks**          | Indentation (significant whitespace)       | Curly braces `{}`                           | Curly braces `{}`                         |
| **Typing**               | Dynamic + Optional static (type hints)     | Static, explicit                            | Static, strict + inference                |
| **Semicolons**           | Not required                                | Required                                    | Required                                  |
| **Variable Declaration** | `x = 5` (no keyword)                       | `int x = 5;` / `var x = 5;` (C#)           | `let x = 5;` or `let mut x = 5;`          |
| **Constants**            | Convention: `UPPER_CASE`                   | `final` / `const` / `readonly`             | `const` or `static`                       |
| **Entry Point**          | Any file, `if __name__ == "__main__":`     | `public static void main(String[] args)`   | `fn main()`                               |

## 2. Functions & Methods

```python
# Python
def add(a: int, b: int = 0) -> int:      # default args, type hints
    return a + b

lambda x: x * 2                           # anonymous functions

def process(*args, **kwargs):             # flexible arguments
    ...
```

### Key Differences:

- Python: First-class functions, easy closures, no method overloading (use defaults/*args)
- Java/C#: Overloading by parameter types, checked exceptions (Java)
- Rust: No default arguments, no overloading; use builder pattern or multiple functions

## 3. Object-Oriented Programming

```python
class Animal:
    def __init__(self, name):             # constructor
        self.name = name                  # no 'this'
    
    def speak(self):                      # self is explicit
        return f"{self.name} says hello"
    
class Dog(Animal):                        # single inheritance (multiple allowed)
    def speak(self):                      # method override
        return f"{self.name} barks"
```

### Major Differences:

- Python: Multiple inheritance, duck typing, `_private` convention only
- Java: Single inheritance + interfaces, strict access modifiers
- C#: Single inheritance + interfaces, properties, records
- Rust: No inheritance; uses Traits + Structs + Composition

## 4. Collections & Data Structures

| Python                  | Java Equivalent               | C# Equivalent               | Rust Equivalent              |
|-------------------------|-------------------------------|-----------------------------|------------------------------|
| `list`                  | `ArrayList<T>`                | `List<T>`                   | `Vec<T>`                     |
| `tuple`                 | N/A (use immutable list)      | `ValueTuple`                | Tuple `(T, U)`               |
| `dict`                  | `HashMap<K,V>`                | `Dictionary<K,V>`           | `HashMap<K,V>`               |
| `set`                   | `HashSet<T>`                  | `HashSet<T>`                | `HashSet<T>`                 |

**Python Idioms:**
```python
lst = [x**2 for x in range(10)]           # list comprehension
d = {k: v*2 for k, v in old_dict.items()}
for key, value in my_dict.items():        # direct unpacking
```

## 5. Error Handling

```python
try:
    risky_operation()
except ValueError as e:
    handle(e)
except Exception:                         # broad catch allowed
    ...
finally:
    cleanup()
```

### Comparisons:

- Java: Checked exceptions (must declare/handle)
- C#: Exceptions with `try-catch-finally`
- Rust: `Result<T, E>` + `?` operator (no exceptions by convention)

## 6. Memory Management & Ownership

| Aspect                | Python                          | Java / C#                       | Rust                              |
|-----------------------|---------------------------------|---------------------------------|-----------------------------------|
| **Memory**            | Automatic GC                    | Automatic GC                    | Ownership + Borrow Checker        |
| **Mutability**        | Mutable by default              | Mutable unless `final`          | Immutable by default (`mut`)      |
| **Null Safety**       | `None` (runtime errors common)  | `null` (NullPointerException)   | `Option<T>` (compile-time safe)   |

## 7. Concurrency & Async

```python
import asyncio

async def main():
    await asyncio.gather(task1(), task2())

# Threading (GIL limits true CPU parallelism)
import threading
import multiprocessing
```

### Key Notes:

- Python: Asyncio (cooperative multitasking), threading (affected by GIL), multiprocessing for true parallelism
- Java/C#: Strong support for threads, tasks, and `async/await`
- Rust: Fearless concurrency through ownership and borrow checker; async via Tokio or async-std

## 8. Modules & Package Management

```python
import numpy as np
from pathlib import Path

# Package management via pip
# pip install requests
# Use requirements.txt or pyproject.toml (modern)
```

### Comparisons:

- Java: Maven / Gradle (pom.xml or build.gradle)
- C#: NuGet packages (.csproj files)
- Rust: Cargo (Cargo.toml) – fully integrated build + package tool

## 9. Common Pythonic Patterns

```python
# Context Managers (resource management)
with open("file.txt", "r") as f:
    data = f.read()

# Properties (getter-like behavior)
class Circle:
    def __init__(self, radius):
        self.radius = radius
    
    @property
    def area(self):
        return 3.14159 * self.radius ** 2

# Generators (lazy evaluation)
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

# Iteration with enumerate and zip
for index, value in enumerate(items):
    ...

for x, y in zip(list1, list2):
    ...
```

### Pythonic Advantages:

- `with` statement for automatic cleanup (vs try-finally in Java/C#)
- Generators for memory-efficient iteration
- Properties provide clean getter/setter syntax without boilerplate

## 10. Quick Reference Table

| Concept                  | Python Syntax                          | Java / C# / Rust Equivalent                    |
|--------------------------|----------------------------------------|------------------------------------------------|
| Private member           | `self._var` (convention)               | `private` keyword                              |
| Interface / Trait        | Abstract Base Class or `Protocol`      | `interface` / `Trait`                          |
| Enum                     | `enum.Enum`                            | `enum` class / `enum`                          |
| Optional value           | `Optional[T]` or `None`                | `Optional<T>` / `T?` / `Option<T>`             |
| Pattern Matching         | `match` / `case` (3.10+)               | Enhanced `switch` / `match`                    |
| Immutable collection     | `tuple` / `frozenset`                  | `final` List / `ImmutableList` / `&[]`         |
| Function as value        | First-class (pass directly)            | Lambda / Method reference                      |

---

**Python Advantages:** Rapid development, readability, vast ecosystem  
**Trade-offs:** Slower execution in CPU-bound tasks, GIL, runtime type safety

**Official Resources:**
- Python Docs: https://docs.python.org/3/





