---
title: "Python Exceptions"
date: 2026-09-15
draft: false
tags: ["Python"]
description: "Một vài kiến thức cơ bản về exception trong python"
---
# Python exceptions

Exceptions có nghĩa là lỗi xảy ra khi chạy chương trình, nó khác với syntax error lúc code.

---
# Raise Exceptions

Thông thường thì python sẽ tự raise exceptions khi nó xảy ra, nhưng mình cần chủ động báo tình huống đó ra để dễ debug hơn. Nó sẽ có nhiều trường hợp exceptions khác nhau, để mà nói cơ bản nhất thì sẽ có ví dụ:
```python
num = 10

if num > 5:
    raise Exception(f"{num} > 5")

print(num)
```

---
# Assert

`assert` cũng có thể tạo ra exceptions, nhưng nó thường dùng để kiểm tra assumption trong lúc development/debugging. Nếu python code chạy với `-O` (optimized mode) thì assert sẽ bị bỏ qua. Kết luận ở đây là raise được sử dụng cho runtime validation, còn assert là cho lúc debug. Ví dụ:
```python
num = 10
assert number > 5, "Number should be > 5"

print(num)
```

---
# Try and Except

Đây là cách thông dụng để chương trình phản ứng với lỗi, ví dụ một đoạn code parse datetime:
```python
from datetime import datetime
from typing import Any

def parse_datetime(value: Any) -> datetime | None:
    try:
        return datetime.fromisoformat(value)
    except (TypeError, ValueError):
        return None
```

Python sẽ chạy try tới khi gặp exception, lúc đó nó sẽ bỏ qua toàn bộ các phần còn lại và nhảy thẳng vào except.

---
# Catch Exception

Có thể lấy exception object bằng `as`. Ví dụ với đoạn code dưới:
```python
try:
    with open("file.log") as file:
        data = file.read()
except FileNotFoundError as error:
    print(error)
```

---
# Bare Except ?

Bare Except nghĩa là Bad Except, ví dụ:
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

viết như thế này nó sẽ bắt mọi exception, kể cả những lỗi mình hoàn toàn không dự kiến khiến việc debug sẽ khó hơn.

---
# Pass Exception ?

Ví dụ:
```python
try:
    ...
except:
    pass
```

là không nên vì dù có gặp exception nó cũng sẽ nuốt mất và chương trình tiếp tục chạy, trong thực tế thường thì sẽ cần log lỗi ra:
```python
logger.exception(...)
```

---
# Else + Try

Khi sử dụng else:
```python
try:
    ...
except:
    ...
else:
    print(...)
```

cụm code sau else sẽ chỉ chạy nếu như khối try chạy hết mà không có exception, khác với việc viết `print(...)` mà không có else, việc viết không có else nghĩa là exception được handle thì nó vẫn sẽ chạy dòng print.

---
# Finally

`finally` dùng cho code phải chạy dù có exception hay không. Ví dụ:
```python
try:
    file = open(data.txt)
except FileNotFoundError:
    print("file not found")
finally:
    print("finished")
```

Trong thực tế, finally có thể sử dụng để cleanup resource sau khi chạy, ví dụ như:
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

Ví dụ với một cấu trúc đầy đủ:
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

Python có nhiều build-in exceptions, nhưng đôi khi application sẽ có lỗi riêng mà không nằm trong mớ build-in này, khi đó có thể tạo custom exception:

```python
import sys


class PlatformException(Exception):
    """Incompatible platform"""

def linux_interaction():
    import os

    if linux not in sys.platform:
        raise PlatformException("Function can only run on Linux system")
```