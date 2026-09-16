---
title: "Python Exceptions"
date: 2026-09-15
draft: false
tags: ["Python"]
description: "Some basic knowledge about exception in python"
---
# Python exceptions

Exceptions mean errors that occur when running the program, they are different from syntax errors while coding.

---
# Raise Exceptions

Normally python will automatically raise exceptions when they occur, but I need to proactively report that situation to make debugging easier. There will be many different exception cases, to put it in the most basic way there will be an example:
```python
num = 10

if num > 5:
    raise Exception(f"{num} > 5")

print(num)
```

---
# Assert

`assert` can also create exceptions, but it is usually used to check assumptions during development/debugging. If python code runs with `-O` (optimized mode) then assert will be ignored. The conclusion here is that raise is used for runtime validation, while assert is for debugging. Example:
```python
num = 10
assert number > 5, "Number should be > 5"

print(num)
```

---
# Try and Except

This is a common way for the program to react to errors, for example a piece of code that parses datetime:
```python
from datetime import datetime
from typing import Any

def parse_datetime(value: Any) -> datetime | None:
    try:
        return datetime.fromisoformat(value)
    except (TypeError, ValueError):
        return None
```

Python will run try until it encounters an exception, at that point it will skip all the remaining parts and jump straight into except.

---
# Catch Exception

The exception object can be obtained using `as`. For example with the code below:
```python
try:
    with open("file.log") as file:
        data = file.read()
except FileNotFoundError as error:
    print(error)
```

---
# Bare Except ?

Bare Except means Bad Except, for example:
```python
try:
    ...
except:
    ...
```
```python
try:
    ...
except Exception:
    ...
```

writing it like this will catch every exception, including errors I completely did not expect making debugging more difficult.

---
# Pass Exception ?

Example:
```python
try:
    ...
except:
    pass
```

is not recommended because even if an exception occurs it will swallow it and the program will continue running, in practice it will usually be necessary to log the error:
```python
logger.exception(...)
```

---
# Else + Try

When using else:
```python
try:
    ...
except:
    ...
else:
    print(...)
```

the code block after else will only run if the try block finishes without any exception, different from writing `print(...)` without else, writing it without else means that if the exception is handled then it will still run the print line.

---
# Finally

`finally` dùng cho code phải chạy dù có exception hay không. Example:
```python
try:
    file = open(data.txt)
except FileNotFoundError:
    print("file not found")
finally:
    print("finished")
```

In practice, finally can be used to clean up resources after running, for example:
```python
connection = None

try:
    connection = connect_database()
except ConnectionError as error:
    print(error)
finally:
    if connection:
        connection.close()
```

---
# Try + Except + Else + Finally

For example with a complete structure:
```python
try:
    num = int(input())
    result = 10 / num
except ValueError:
    print("num must be a number")
except ZeroDivisionError:
    print("num cannot be zero")
else:
    print(result)
finally:
    print("finished")
```

---
# Custom Exception

Python has many build-in exceptions, but sometimes an application will have its own errors that are not in this set of build-ins, in that case a custom exception can be created:

```python
import sys


class PlatformException(Exception):
    """Incompatible platform"""

def linux_interaction():
    import os

    if linux not in sys.platform:
        raise PlatformException("Function can only run on Linux system")
```