---
title: "Pandas"
date: 2026-09-21
draft: true
tags: ["Pandas"]
description: "Some basic knowledge about pandas"
---
# Pandas

So what is `pandas` here - `pandas` is a python library used to work with tabular data, it helps me read, analyze, clean, transform and prepare data.

Syntax import:
```python
import pandas as pd
```

---
# Series

Series is like a column with an index. for example:
```python
import pandas as pd

s = pd.Series([10,20,30])
```

Will give the result:
```text
0 10
1 20
2 30
```

With series I can set the index myself:
```python
import pandas as pd

s = pd.Series([10,20,30], index=["a","b","c"])
```

Series will usually appear when I take a column from DataFrame `df["price]`.

---
# DataFrame

DataFrame is a data table consisting of row and column., for example created from dict:
```python
data = {
    "name": ["Cece","Sage"],
    "age": [25,24],
    "city": ["HCM","HN"]
}

df = pd.DataFrame(data)
```

The result will be:

|    | name | age | city |
|:---|:-----|:----|:-----|
| 0  | Cece | 25  | HCM  |
| 1  | Sage | 24  | HN   |

Some basic information:
```python
df.shape
df.columns
df.index
df.dtypes
```

- `df.shape` returns tuple(rows, columns), for example `(1,2)`.
- `df.columns` returns an index containing a list of all column names.
- `df.index` returns index information of the rows, it can be understood that it will print start, stop and step.
- `df.dtypes` returns a series displaying the data type of each column

---
# Read data

`CSV`
```python
df = read.read_csv("data.csv")
```

`JSON`
```python
df = pd.read_json("data.json")
```

Detailed example:
```python
import pandas as pd

input_path = ...

df = pd.read_csv(
    input_path,
    sep=";",
    encoding="utf-8",
    usecols=["id","name","price"],
    nrows=100
)
```
```python
import pandas as pd

input_path = ...

df = pd.read_json(
    "SELECT * FROM table",
    connection
)
```

---
# Quick Lookup Data

```python
df.head()
df.head(10)
df.tail()
df.info()
df.describe()
```

Corresponding to the code lines, I will view the first 5 rows, the first 10 rows, the last 5 rows, view information about columns, non-null count, dtype, memory and finally view numeric statistics. Note here that `info` will default to `sys.stdout` so there is no need to use the `print` function to print the result, while `describe` will return a df.

Normally a large df will not be fully printed to the screen, at that time I can use `df.to_string()` to print everything, but it is not highly recommended.

---
# Chose Data

There will be two ways to select data:
```python
df = ["love"]
df = [["love","hate"]]
```

corresponding to each statement,  1 will return Series, 2 will return DataFrame.

---
# loc and iloc

`loc` selects by label, while `iloc` selects by position.
```python
df.loc[0]
df.loc[0, "name"]
df.loc[0:2, ["name", "age"]]
```

Note: `0:2` will also include the row with index 2, different from `.iloc`

```python
df.iloc[0]
df.iloc[0, 1]
df.iloc[0:3, 0:2]
```

To make it easy to distinguish, it can be summarized that `.loc` uses purely names/labels, while `.iloc` uses purely numeric positions.

---
# Filter Data

Based on condition:
```python
df[df["age]"] > 20]
```

multiple conditions:
```python
df[
    (df["age"] > 30) & (df["city"] == "HCM")
]
# or
df[
    (df["city"] == "HN") | (df["city"] == "HCM")
]
```

Check whether it is in the list:
```python
df[df["city"].isin[("HCM", "HN")]]
```

String filter:
```python
df[df["name"].str.contains("Ali", na=False)]
```

Null filter:
```python
df[df["age"].isna()]
# or
df[df["age"].notna()]
```

---
# Sort Data

By one column:
```python
df.sort_values("age")
# or
df.sort_values(["age","city"], ascending=[True,False])
```

By index:
```python
df.sort_index()
```

---
# Clean Data

Missing Values:
```python
# Check
df.isna()

# Count -> returns a series displaying the total number of cells that are NaN in each column
df.isna().sum()

# Delete
df.dropna()
df.dropna(subset=["price"]) # only delete rows where price is NaN

# Fill value
df["price"] = df["price"].fillna(0)
df["price"] = df["price"].fillna(df["price"].mean())
```

Duplicate Values:
```python
# Check
df.duplicated()

# Count
df.duplicated().sum()

# Delete
df.drop_duplicates()
df.drop_duplicates(subset=["id"])
```

---
# Dtype and Change Data type

```python
# Check
df.dtypes
```

Convert type:
```python
df["age"] = df["age"].astype(int)
```

or a safer way can be used:
```python
df["price"] = pd.to_numeric(df["price"], errors="coerce")
df["date"] = pd.to_datetime(df["date"], errors="coerce")
```

Code explanation: `coerce` is used to turn junk data types into `NaN`, `raise` can be used to stop the program or `ignore` to keep that value unchanged.

---
# Columns

Create a new column:
```python
df["total"] = df["price"] * df["quantity"]
```

Replace value:
```python
df["city"] = df["city"].replace("HCM", "HN")
```

Rename:
```python
df = df.rename(columns={"customer_name": "name"})
```

Delete column:
```python
df = df.drop(columns=["unused_column"])
```

Delete row:
```python
df = df.drop(index=0)
```

---
# Apply, map and vectorized operations

Normally when working with columns, prioritize operating directly on the column instead of using loop, for example:
```python
df["price_with_tax"] = (df["price"] * 1.1)
```

By nature pandas is designed to operate by column/vector. In case a custom function is needed, it can be used:
```python
def categorize(age):
    if age >= 18:
        return "adult"
    return "minor"

df["category"] = df["age"].apply(categorize)
```

or direct mapping:
```python
mapping = {
    "HCM": "South",
    "HN": "North"
}

df["region"] = df["city"].map(mapping)
```

Note: normally if the rule is simple it is better to use vectorized when possible, `apply()` works well but is not always the fastest choice.

---
# GroupBy and Aggeration

Given an example as follows:
```python
df.groupby("category")["sales"].sum()
```

The meaning of the following code is to group by category, take sales and sum it, finally creating a new table.

Example using multiple aggregation:
```python
df.groupby("category")["sales"].agg(["sum", "mean", "count"])
```

or multiple columns:
```python
result = df.groupby("category").agg(
    total_sales=("sales", "sum"),
    avg_price=("price", "mean"),
    count=("id", "count")
)
```

Normally after running, the column defined in `groupby` will become the index, at this point it can be used:
```python
result = result.reset_index()
```

to separate the column out and return the normal index column at the beginning of the table.

---
# Merge / Join

For example I have two tables `customers` and `orders`, I will join by:
```python
result = pd.merge(
    orders,
    customers,
    on="customer_id",
    how="left"
)
```

if you want to join with multiple keys, use `on=["id", "date"]`. If the key has different names in the two tables you can use:
```python
pd.merge(
    df1,
    df2,
    left_on="customer_id",
    right_on="id"
)
```

---
# Concatenate

If you want to concatenate 2 rows together, use syntax:
```python
df = pd.concat(
    [df1, df2],
    ignore_index=True
)
```

If you want to concatenate 2 columns together, use syntax:
```python
pd.concat(
    [df1, df2],
    axis=1
)
```

---
# Pivot Table

Pivot is used to reshape and summarize data. For example:
```python
pd.pivot_table(
    df,
    values="sales",
    index="category",
    columns="years",
    aggfunc="sum"
)
```

Example with a specific table:

| category | years | sales |
|----------|-------|-------|
| Shirt    | 2023  | 100   |
| Shirt    | 2023  | 150   |
| Shirt    | 2024  | 200   |
| Pants    | 2023  | 300   |
| Pants    | 2024  | 400   |


after running it will give a new result:

| category | 2023 | 2024 |
|----------|------|------|
| Shirt    | 250  | 200  |
| Pants    | 300  | 400  |

When mentioning pivot, there will be two types which are `pivot` and `pivot_table`. With `df.pivot()` it simply rotates the table without calculation, if the original data has 2 or more duplicate rows, for example category or years then pivot will throw an error. With the other one it will be safer because it has `aggfunc`, when encountering duplicate rows, it will automatically use the function to combine them into 1 single number.

Specific example:
```python
pd.pivot_table(
    df,
    values="sales",
    index="category",
    columns="years",
    aggfunc="sum",
    fill_value=0,   # Fill number 0 into missing cells (NaN) instead of leaving them blank
    margins=True    # Automatically add a total column at the end (cumulative)
)
```
---
# Datetime

```python
df["date"] = pd.to_datetime(         # convert data type
    df["date"]
)

df["year"] = df["date"].dt.year      # Get year information, similarly for month, day, dayname
df["month"] = df["date"].dt.month
df["day"] = df["date"].dt.day
df["weekday"] = (
    df["date"].dt.day_name()
)

# filter date
df[
    df["date"]
    >= "2026-01-01"
]
```
---
# String Operations

With a string column, I will use `.str`:
```python
df["name"] = (
    df["name"].str.lower()
)

df["name"] = (
    df["name"].str.strip()
)

df[
    df["name"]
    .str.contains("john", case=False, na=False)
]

df["phone"] = (
    df["phone"]
    .str.replace("-", "", regex=False)
)
```

---
# Unique and Frequency

Unique values:
```python
# returns an array containing non-duplicate category names
df["category"].unique()

# returns 1 single integer (not counting NaN), for example returning 2 means there are a total of 2 unique values
df["category"].nunique()
```

Frequency:
```python
# Count xem mỗi giá trị xuất hiện bao nhiêu lần trong cột và tự động sắp xếp từ nhiều nhất đến ít nhất
# The normalize variable changes the count into a percentage
df["category"].value_counts(normalize=True)
```

---
# Export data

```python
df.to_csv(
    "output.csv",
    index=False
)

df.to_excel(
    "output.xlsx",
    index=False
)

df.to_json(
    "output.json",
    orient="records"
)

df.to_sql(
    "table_name",
    connection,
    if_exists="append",
    index=False
)
```

---
# Workflow

```python
import pandas as pd

# load
df = pd.read_csv("Litaaya.csv")

# inspect
print(df.head())
print(df.info())
print(df.isna().sum())

# clean
df = df.drop_duplicates()

df["date"] = pd.to_datetime(
    df["date"],
    errors="coerce"
)

# transform
df["year"] = df["date"].dt.year

# filter
df = df[
    df["sales"] > 0
]

# aggregate
summary = (
    df.groupby("category").agg(
        total_sales=("sales", "sum"),
        avg_sales=("sales", "mean"),
        count=("sales", "count")
    ).reset_index()
)

# sort
summary = summary.sort_values(
    "total_sales",
    ascending=False
)

# export
summary.to_csv(
    "summary.csv",
    index=False
)
```

---
# Cheatsheet

| Purpose            | Syntax                          |
|--------------------|---------------------------------|
| Import             | `import pandas as pd`           |
| Create DataFrame   | `pd.DataFrame(data)`            |
| Read CSV           | `pd.read_csv("file.csv")`       |
| Read Excel         | `pd.read_excel("file.xlsx")`    |
| Read JSON          | `pd.read_json("file.json")`     |
| View first         | `df.head()`                     |
| View last          | `df.tail()`                     |
| Information        | `df.info()`                     |
| Statistics         | `df.describe()`                 |
| Size               | `df.shape`                      |
| Columns            | `df.columns`                    |
| Dtypes             | `df.dtypes`                     |
| One column         | `df["col"]`                     |
| Multiple columns   | `df[["a", "b"]]`                |
| Label selection    | `df.loc[...]`                   |
| Position selection | `df.iloc[...]`                  |
| Filter             | `df[df["age"] > 18]`            |
| AND                | `(cond1) & (cond2)`             |
| OR                 | `(cond1) \| (cond2)`            |
| In list            | `df["col"].isin([...])`         |
| Sort               | `df.sort_values("col")`         |
| Missing            | `df.isna()`                     |
| Count missing      | `df.isna().sum()`               |
| Fill missing       | `df["col"].fillna(value)`       |
| Drop missing       | `df.dropna()`                   |
| Duplicates         | `df.duplicated()`               |
| Drop duplicates    | `df.drop_duplicates()`          |
| Convert numeric    | `pd.to_numeric()`               |
| Convert datetime   | `pd.to_datetime()`              |
| Convert dtype      | `df["col"].astype(...)`         |
| Rename             | `df.rename(columns={...})`      |
| Drop column        | `df.drop(columns=[...])`        |
| New column         | `df["new"] = ...`               |
| Replace            | `df["col"].replace(...)`        |
| Apply              | `df["col"].apply(func)`         |
| Unique             | `df["col"].unique()`            |
| Count unique       | `df["col"].nunique()`           |
| Frequency          | `df["col"].value_counts()`      |
| Group              | `df.groupby("col")`             |
| Aggregate          | `.agg(...)`                     |
| Merge              | `pd.merge(df1, df2, ...)`       |
| Concatenate        | `pd.concat([df1, df2])`         |
| Pivot              | `pd.pivot_table(...)`           |
| Mean               | `df["col"].mean()`              |
| Sum                | `df["col"].sum()`               |
| Min/Max            | `.min()` / `.max()`             |
| Correlation        | `df.corr(numeric_only=True)`    |
| Export CSV         | `df.to_csv(..., index=False)`   |
| Export Excel       | `df.to_excel(..., index=False)` |