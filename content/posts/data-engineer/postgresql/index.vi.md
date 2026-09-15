---
title: "PostgreSQL"
date: 2026-09-15
draft: true
tags: ["PostgreSQL"]
description: "Một vài thông tin cơ bản về postgresql"
---
> ref: https://neon.com/postgresql/python
> Mình đã sử dụng Postgresql trong quá trình học và làm việc khá nhiều nhưng đa phần là làm lúc bắt đầu luôn nên mấy cái cơ bản của postgres mình không nắm vững lắm. Blog này nhằm mục đích tóm tắt cơ bản về postgres để mốt lôi ra đọc cho đỡ quên.

---
# Table of content

---
# PostgreSQL

`PostgresSQL` là một hệ quản trị cơ sở dữ liệu quan hệ mã nguồn mở, nó lưu dữ liệu theo dạng `db`-> `tables` -> `rows + columns`.

`postgres` hỗ trợ sql và có các tính năng như `primary key`, `foreign key`, `constraints`, etc.

Trong python, mình sẽ không thao tác trực tiếp với `postgres` bằng sql terminal, mà sẽ xài một db driver để python gửi sql tới `postgres`. Có các phương thức cơ bản có thể kể đến như `psycopg3`, `pscopg2`, `asyncpg`.

---
# Connect Python PostgreSQL

Với python hiện tại, có thể xài `psycopg` hoặc bản binary của nó, cách cũ hơn thì có `psycopg2`, những cái này gọi là driver:
```shell
pip install psycopg
pip install psycopg[binary]
pip install psycopg2-binary
```

Driver sẽ chịu trách nhiệm mở connect, gửi sql, nhận kết quả, quản lý transaction, đóng connection.

Để python kết nối postgres, nó sẽ cần các thông tin cơ bản là:
```text
host
port
database
user
password
```

Với postgres mặc định, port mặc định là `5432`.

Có thể lưu riêng:
```python
host = "localhost"
database = "mydb"
user = "Litaaya"
password = "1234"
```

hoặc dùng một connection string:
```text
postgresql://user:password@host/database
```

Về vấn đề mật khẩu, không nên hard-core password vào trong source code, thông thường mình sẽ lưu nó trong `environment variable` hoặc `.env`
```python
import os
from dotenv import load_dotenv

load_dotenv()

conn_string = os.getenv("database_url")
```

rồi sau đó:
```python
import psycopg

conn = psycopg.connect(conn_string)
```

Quay lại với connection, đầu tiên mình sẽ connect sử dụng `psycopg`, và để gửi sql, mình sẽ tạo `cursor`, có thể hình dụng connection -> cursor -> execute sql -> postgresql:
```python
import os
import psycopg
from dotenv import load_dotenv

with psycopg.connect(conn_string) as conn:
    with conn.cursor() as cur:
        cur.execute("select 1;")
        print(cur.fetchone())
```

---
# Create/Insert Tables

Vì ví dụ trong ref mình đọc thấy hơi dài dòng nên mình sẽ tự viết một ví dụ ngắn, mình sẽ giả sử như project có `.env`:
```text
database_url=postgresql://user:password@host/database
```
```python
import os
import psycopg
from dotenv import load_dotenv

load_dotenv()
database_url = os.getenv("database_url")

with psycopg.connect(database_url) as conn:
    conn.execute("""
        create table if not exists books (
            id serial primary key,
            title varchar(255) not null,
            author varchar(255),
            publication_year integer,
            in_stock boolean default True,
        )
    """)

    conn.execute(
        """
        insert into books(
            title,
            author,
            publication_year,
            in_stock
        )
        values (%s, %s, %s, %s)
        """,
        ("The Hobbit", "J.R.R. Tolkien", 1937, True)
    )
```

Ở trên mình có nói tới việc tạo cursor nhưng trong đoạn code, vì sử dụng `psycopg3` nên có thể `conn.execute(...)` thay vì `cur = conn.cursor()` là vì `connect.execute()` sẽ tự tạo cursor, execute query và trả cursor đó về.

Ở trong đoạn code trên, parameter `values (%s, %s, %s, %s)` sẽ nhận được data ở dưới, nó sẽ được truyền riêng nh vậy thay vì `f"insert into books values ('{title}')"`, để cho dễ hiểu mình sẽ đặt hai đoạn code cạnh nhau:
```python
title = "The Hobbit"
author = "J.R.R. Tolkien"
year = 1937
in_stock = True

sql_query = f"""
    insert into books(
            title,
            author,
            publication_year,
            in_stock
        )
    values ('{title}', '{author}', {year}, {in_stock})
"""
conn.execute(sql_query)

conn.execute(
    """
    insert into books(
            title,
            author,
            publication_year,
            in_stock
        )
        values (%s, %s, %s, %s)
    """,
    (title, author, year, in_stock)
)
```

Giải thích nhanh lý do sử dụng `%s` là vì nó chống tấn công sql injection, việc dùng f-string sẽ khiến sql bị vỡ cú pháp, ngược lại khi truyền dưới dạng tham số tuple, thư viện csdl sẽ tự xử lý các ký tự nguy hiểm. Ngoài ra, thư viện sẽ tự chuyển đổi kiểu dữ liệu từ python sang sql chuẩn, ví dụ `True` thành `TRUE` hoặc `1` trong csdl, chuỗi chữ tự được bọc trong `...`, số nguyên được giữ kiểu numeric.

---
# CRUD

Bây giờ tới thao tác tổng thể cho một chương trình hoàn chỉnh:
```python
import os
import psycopg
from dotenv import load_dotenv

load_dotenv()
database_url = os.getenv("database_url")

with psycopg.connect(database_url) as conn:
    # Create
    conn.execute("""
        CREATE TABLE IF NOT EXISTS books (
            id SERIAL PRIMARY KEY,
            title VARCHAR(255) NOT NULL,
            author VARCHAR(255),
            publication_year INTEGER,
            in_stock BOOLEAN DEFAULT TRUE
        )
    """)
    
    # Insert
    conn.execute(
        """
        INSERT INTO books (
            title,
            author,
            publication_year,
            in_stock
        )
        VALUES (%s, %s, %s, %s)
        """,
        ("The Hobbit", "J.R.R. Tolkien", 1937, True)
    )
    
    # Select
    rows = conn.execute("""
        SELECT
            id,
            title,
            author,
            publication_year,
            in_stock
        FROM books
        ORDER BY id
    """).fetchall()
    
    # Update
    conn.execute(
        """
        UPDATE books
        SET in_stock = %s
        WHERE title = %s
        """,
        (False, "The Hobbit")
    )

    # Delete
    conn.execute(
        """
        DELETE FROM books
        WHERE title = %s
        """,
        ("The Hobbit",)
    )
```

---
# Select

Ví dụ:
```python
rows = conn.execute(
    """
    select id, title, author
    from books
    where publication_year > %s
    """,
    (1950,)
).fetchall()

for row in rows:
    print(row)
```

Mỗi row mặc định trả về dạng tuple ví dụ như `[(1, Dune, 'Frank Herbert)]`, nó sẽ các các dạng mặc định là:
```text
fetchone() : một rows
fetchmany() : một số rows
fetchall() : toàn bộ rows
```

Ngoài ra, nên lưu ý về RAM khi sử dụng `fetchall`, có thể duyệt trực tiếp thay vì fetchall, ví dụ như:
```python
with psycopg.connect(database_url) as conn:
    cur = conn.execute("""
        select id, title
        from books
        order by id
    """)
    
    for row in cur:
        print(row)
```

---
# Insert

Khi có nhiều records ví dụ như:
```python
books = [
    ("The Hobbit", "J.R.R. Tolkien", 1937, True),
    ("1984", "George Orwell", 1949, True),
    ("Dune", "Frank Herbert", 1965, True),
]
```

Có thể dùng cursor và `executemany()`:
```python
import os
import psycopg
from dotenv import load_dotenv

load_dotenv
database_url = os.getenv("database_url")

books = [
    ("The Hobbit", "J.R.R. Tolkien", 1937, True),
    ("1984", "George Orwell", 1949, True),
    ("Dune", "Frank Herbert", 1965, True),
]

with psycopg.connect(database_url) as conn:
    with conn.cursor() as cur:
cur.executemany(
    """
    insert into books (
        title,
        author,
        publication_year,
        in_stock
    )
    values (%s, %s, %s, %s)
    """,
    books
)
```

---
# Update/Delete

Với psycopg 3, có thể sử dụng rowcount để kiểm tra số row bị ảnh hưởng khi update và delete:
```python
with psycopg.connect(database_url) as conn:
    cur = conn.execute(
        """
        update books
        set in_stock = %s
        where id = %s
        """,
        (False, 1)
    )

    print(cur.rowcount)
```

---
# Transaction in Psycopg 3

Psycopg3 sẽ quản lý transaction, nếu block chạy thành công, nó sẽ commit, nếu có exception thì sẽ rollback và connection cũng được đóng. Hiểu nôm na là success hết thì commit, một cái error là rollback hết.

---
# Cursor When ?

Trong psycopg 3, các query đơn giản có thể sử dụng `conn.execute()`, nhưng cursor riêng vẫn sẽ tốt khi nhiều operations thuộc cùng một đoạn logic thay vì để nó tự tạo, có thể ví dụ như:
```python
with psycopg.connect(DATABASE_URL) as conn:
    with conn.cursor() as cur:
        cur.execute(
            """
            SELECT id, title
            FROM books
            WHERE in_stock = %s
            """,
            (True,)
        )

        rows = cur.fetchall()
```

Nên sử dụng consor khi xử lý tập dữ liệu lớn, cần kiểm soát cách lấy dữ liệu hoặc thực hiện chuỗi lệnh sql liên tiếp trong 1 transaction.

---
# Codebase

Cho một ví dụ cụ thể như sau:
```text
project/
    .env
    main.py
    db.py
```

`db.py`:
```python
import os
import psycopg
from dotenv import load_dotenv

load_dotenv()

db_url = os.getenv("db_url")

def create_table():
    ...

def add_book(title, author, publication_year, in_stock=True):
    ...

def get_books():
    ...

def update_stock(book_id, in_stock):
    ...

def delete_book(book_id):
    ...
```

`main.py`:
```python
from db import (
    create_table,
    add_book,
    get_books,
    update_stock,
    delete_book,
)

create_table()

add_book("Dune", "Frank Herbert", 1965)

books = get_books()
```

---
# PostgreSQL Functions

`Postgresql Functions` là function được định nghĩa và chạy bên trong postgresql thay vì logic nằm trong python, ví dụ với một function:
```postgres-sql
create function add_numbers(a integer, b integer)
return integer
as $$
begin
    return a + b;
end;
$$ language plpsql;
```

Từ python, gọi bằng query:
```python
with psycopg.connect(database_url) as conn:
    result = conn.execute(
        "select add_number(%s, %s)",
        (10, 20)
    ).fetchone()
```

kết quả trả về sẽ là `(30,)`.

---
# PostgreSQL Stored Procedures

Cái này giống với function ở chỗ logic nằm trong db, nhưng khác cách gọi, ví dụ mình có:
```postgres-sql
create procedure mark_book_out_of_stock(book_id integer)
language plpgsql
as $$
begin
    update books
    set in_stock = false
    where id = book_id;
end;
$$;
```

`python`:
```python
with psycopg.connect("database_url") as conn:
    conn.execute(
        "call mark_book_out_of_stock(%s)",
        (1,)
    )
```

Để dễ phân biệt, có thể hiểu function sẽ thường cần return value, procedure thường dùng để thực hiện một nhóm thao tác.

---
# BLOB / Binary Data

`BLOB` nghĩa là `Binary Large Object`, tức là dữ liệu nhị phân như ảnh, pdf, audio, file nén, etc. chứ không phải text hay number. Trong postgres khi làm với python, cách dễ hiểu nhất là dùng kiểu `BYTEA`.

Ví dụ có:
```postgres-sql
create table files (
    id serial primary key,
    filename varchar(255),
    data bytea
);
```

Ở đây data sẽ có nội dung binary của file, vậy bây giờ để psycopg 3 có thể gửi nó vào postgres, cần:
```python
with open("image.png", "rb") as file:
    binary_data = file.read()

with psycopg.connect(database_url) as conn:
    conn.execute(
        """
        insert into files(filename, data)
        values (%s, %s)
        """,
        ("image.png", binary_data)
    )
```

và lấy ra + ghi lại:
```python
with psycopg.connect(database_url) as conn:
    row = conn.execute(
        """
        select filename, data
        from files
        where id = %s
        """,
        (1,)
    ).fetchone()

filename, binary_data = row

with open(filename, "wb") as file:
    file.write(binary_data)
```