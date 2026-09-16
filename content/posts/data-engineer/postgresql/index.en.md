---
title: "PostgreSQL"
date: 2026-09-15
draft: false
tags: ["PostgreSQL"]
description: "Some basic information about postgresql"
---
> ref: https://neon.com/postgresql/python
> I have used Postgresql quite a lot during studying and working but most of the time I just started using it right away so I do not understand the basics of postgres very well. This blog aims to summarize the basics of postgres so I can pull it out to read later and avoid forgetting.

---
# Table of content

---
# PostgreSQL

`PostgresSQL` is an open-source relational database management system, it stores data in the form of `db`-> `tables` -> `rows + columns`.

`postgres` supports sql and has features such as `primary key`, `foreign key`, `constraints`, etc.

In python, I will not interact directly with `postgres` through the sql terminal, but will use a db driver so python can send sql to `postgres`. Some basic methods can be mentioned such as `psycopg3`, `pscopg2`, `asyncpg`.

---
# Connect Python PostgreSQL

With python currently, `psycopg` or its binary version can be used, the older way is `psycopg2`, these are called drivers:
```shell
pip install psycopg
pip install psycopg[binary]
pip install psycopg2-binary
```

The driver will be responsible for opening the connection, sending sql, receiving results, managing transactions, closing the connection.

For python to connect to postgres, it will need the basic information:
```text
host
port
database
user
password
```

With postgres by default, the default port is `5432`.

Can be stored separately:
```python
host = "localhost"
database = "mydb"
user = "Litaaya"
password = "1234"
```

or use a connection string:
```text
postgresql://user:password@host/database
```

Regarding passwords, password should not be hard-coded into the source code, normally I will store it in an `environment variable` or `.env`
```python
import os
from dotenv import load_dotenv

load_dotenv()

conn_string = os.getenv("database_url")
```

then after that:
```python
import psycopg

conn = psycopg.connect(conn_string)
```

Back to the connection, first I will connect using `psycopg`, and to send sql, I will create a `cursor`, it can be visualized as connection -> cursor -> execute sql -> postgresql:
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

Because the example in the ref I read seems a bit lengthy, I will write a short example myself, I will assume the project has `.env`:
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

Above I mentioned creating a cursor but in the code, because `psycopg3` is used, `conn.execute(...)` can be used instead of `cur = conn.cursor()` because `connect.execute()` will automatically create a cursor, execute the query and return that cursor.

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

A quick explanation for using `%s` is because it prevents sql injection attacks, using f-string will cause sql syntax to break, in contrast when passing it as tuple parameters, the database library will automatically handle dangerous characters. In addition, the library will automatically convert data types from python to standard sql, for example `True` to `TRUE` or `1` in the database, strings are automatically wrapped in `...`, integers keep the numeric type.

---
# CRUD

Now moving on to the overall operations for a complete program:
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

Example:
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
fetchone() : one row
fetchmany() : some rows
fetchall() : all rows
```

In addition, RAM should be considered when using `fetchall`, it is possible to iterate directly instead of fetchall, for example:
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

When there are many records for example:
```python
books = [
    ("The Hobbit", "J.R.R. Tolkien", 1937, True),
    ("1984", "George Orwell", 1949, True),
    ("Dune", "Frank Herbert", 1965, True),
]
```

Cursor and `executemany()` can be used:
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

With psycopg 3, rowcount can be used to check the number of rows affected when updating and deleting:
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

Psycopg3 will manage the transaction, if the block runs successfully, it will commit, if there is an exception it will rollback and the connection will also be closed. Simply put, if everything succeeds then commit, if one thing errors then rollback everything.

---
# Cursor When ?

In psycopg 3, simple queries can use `conn.execute()`, but a separate cursor will still be better when multiple operations belong to the same block of logic instead of letting it create one automatically, for example:
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

A consor should be used when processing large datasets, when needing to control how data is fetched or execute a sequence of sql commands consecutively in 1 transaction.

---
# Codebase

For a specific example as follows:
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

`Postgresql Functions` is a function defined and run inside postgresql instead of having the logic in python, for example with a function:
```postgres-sql
create function add_numbers(a integer, b integer)
return integer
as $$
begin
    return a + b;
end;
$$ language plpsql;
```

From python, call it with a query:
```python
with psycopg.connect(database_url) as conn:
    result = conn.execute(
        "select add_number(%s, %s)",
        (10, 20)
    ).fetchone()
```

the returned result will be `(30,)`.

---
# PostgreSQL Stored Procedures

This is similar to a function in that the logic is inside the db, but the way it is called is different, for example I have:
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

To make it easier to distinguish, it can be understood that a function will usually need to return a value, a procedure is usually used to perform a group of operations.

---
# BLOB / Binary Data

`BLOB` means `Binary Large Object`, which is binary data such as images, pdf, audio, compressed files, etc. rather than text or numbers. In postgres when working with python, the easiest way to understand it is to use the `BYTEA` type.

For example:
```postgres-sql
create table files (
    id serial primary key,
    filename varchar(255),
    data bytea
);
```

Here data will contain the binary content of the file, so now for psycopg 3 to be able to send it into postgres, it is necessary to:
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

and retrieve it + write it back:
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