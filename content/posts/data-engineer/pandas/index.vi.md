---
title: "Pandas"
date: 2026-09-21
draft: false
tags: ["Pandas"]
description: "Một vài kiến thức cơ bản về pandas"
---
# Pandas

Vậy `pandas` ở đây là gì - `pandas` là thư viện của python dùng để làm việc với dữ liệu dạng bảng, nó giúp mình đọc, phân tích, làm sạch, biến đổi và chuẩn bị dữ liệu.

Syntax import:
```python
import pandas as pd
```

---
# Series

Series giống như một column có index. ví dụ như:
```python
import pandas as pd

s = pd.Series([10,20,30])
```

Sẽ cho ra kết quả:
```text
0 10
1 20
2 30
```

Với series mình có thể tự đặt index:
```python
import pandas as pd

s = pd.Series([10,20,30], index=["a","b","c"])
```

Series thường sẽ xuất hiện khi mình lấy một column từ DataFrame `df["price]`.

---
# DataFrame

DataFrame là bảng dữ liệu gồm row và column., ví dụ tạo từ dict:
```python
data = {
    "name": ["Cece","Sage"],
    "age": [25,24],
    "city": ["HCM","HN"]
}

df = pd.DataFrame(data)
```

Kết quả sẽ là:

|    | name | age | city |
|:---|:-----|:----|:-----|
| 0  | Cece | 25  | HCM  |
| 1  | Sage | 24  | HN   |

Một vài thông tin cơ bản:
```python
df.shape
df.columns
df.index
df.dtypes
```

- `df.shape` trả về tuple(rows, columns), ví dụ như `(1,2)`.
- `df.columns` trả về một index chứa danh sách tên tất cả các cột.
- `df.index` trả về thông tin index của các dòng, có thể hiểu nó sẽ in ra start, stop và step.
- `df.dtypes` trả về một series hiển thị kiểu dữ liệu của từng cột

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

Ví dụ chi tiết:
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

Tương ứng với các dòng code, mình sẽ coi 5 dòng đầu, 10 dòng đầu, 5 dòng cuối, coi thông tin về columns, non-null count, dtype, memory và cuối cùng là coi thống kê numeric. Lưu ý chỗ này là `info` sẽ mặc định là `sys.stdout` nên không cần xài hàm `print` để in kết quả ra, còn `describe` sẽ trả về một df.

Thông thường một df lớn sẽ không được in hết ra màn hình, lúc đó mình có thể sử dụng `df.to_string()` để in toàn bộ, nhưng không khuyến cáo lắm.

---
# Chose Data

Sẽ có hai kiểu chọn dữ liệu:
```python
df = ["love"]
df = [["love","hate"]]
```

tương ứng với mỗi câu lệnh,  1 sẽ trả về Series, 2 sẽ trả về DataFrame.

---
# loc and iloc

`loc` chọn theo label, còn `iloc` chọn theo position.
```python
df.loc[0]
df.loc[0, "name"]
df.loc[0:2, ["name", "age"]]
```

Lưu ý: `0:2` sẽ lấy luôn cả dòng index 2, khác với `.iloc`

```python
df.iloc[0]
df.iloc[0, 1]
df.iloc[0:3, 0:2]
```

Để dễ phân biệt, có thể tóm tắt ràng `.loc` sử dụng thuần tên/nhãn, còn `.iloc` thì thuần vị trí số.

---
# Filter Data

Dựa trên điều kiện:
```python
df[df["age]"] > 20]
```

nhiều điều kiện:
```python
df[
    (df["age"] > 30) & (df["city"] == "HCM")
]
# or
df[
    (df["city"] == "HN") | (df["city"] == "HCM")
]
```

Kiểm tra nằm trong danh sách:
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

Theo một column:
```python
df.sort_values("age")
# or
df.sort_values(["age","city"], ascending=[True,False])
```

Theo index:
```python
df.sort_index()
```

---
# Clean Data

Missing Values:
```python
# Kiểm tra
df.isna()

# Đếm -> trả về một series hiện thị tổng ô số bị NaN của từng cột
df.isna().sum()

# Xóa
df.dropna()
df.dropna(subset=["price"]) # dòng nào có price bị NaN mới xóa

# Điền giá trị
df["price"] = df["price"].fillna(0)
df["price"] = df["price"].fillna(df["price"].mean())
```

Duplicate Values:
```python
# Kiểm tra
df.duplicated()

# Đếm
df.duplicated().sum()

# Xóa
df.drop_duplicates()
df.drop_duplicates(subset=["id"])
```

---
# Dtype and Change Data type

```python
# Kiểm tra
df.dtypes
```

Chuyển kiểu:
```python
df["age"] = df["age"].astype(int)
```

hoặc có thể sử dụng cách an toàn hơn:
```python
df["price"] = pd.to_numeric(df["price"], errors="coerce")
df["date"] = pd.to_datetime(df["date"], errors="coerce")
```

Giải thích code: `coerce` để biến các kiểu dữ liệu rác thành `NaN`, có thể sử dụng `raise` để dừng chương trình hoặc `ignore` để giữ nguyên giá trị đó.

---
# Columns

Tạo column mới:
```python
df["total"] = df["price"] * df["quantity"]
```

Thay value:
```python
df["city"] = df["city"].replace("HCM", "HN")
```

Rename:
```python
df = df.rename(columns={"customer_name": "name"})
```

Xóa column:
```python
df = df.drop(columns=["unused_column"])
```

Xóa row:
```python
df = df.drop(index=0)
```

---
# Apply, map and vectorized operations

Thông thường khi thao tác với các cột, ưu tiên thao tác trực tiếp trên column thay vì sử dụng loop, ví dụ như:
```python
df["price_with_tax"] = (df["price"] * 1.1)
```

Bản chất pandas được thiết kể để thao tác theo column/vector. Trong trường hợp nếu cần custom function, có thể sử dụng:
```python
def categorize(age):
    if age >= 18:
        return "adult"
    return "minor"

df["category"] = df["age"].apply(categorize)
```

hoặc mapping trực tiếp:
```python
mapping = {
    "HCM": "South",
    "HN": "North"
}

df["region"] = df["city"].map(mapping)
```

Lưu ý: thông thường nếu rule đơn giản nên biết vectorized khi có thể, `apply()` sử dụng tốt nhưng không phải lúc nào cũng là lựa chọn nhanh nhất.

---
# GroupBy and Aggeration

Cho một ví dụ như sau:
```python
df.groupby("category")["sales"].sum()
```

Ý nghĩa của đoạn code sau là sẽ group theo category, lấy sales và sum lại, cuối cùng là tạo ra một bảng mới.

Ví dụ sử dụng nhiều aggregation:
```python
df.groupby("category")["sales"].agg(["sum", "mean", "count"])
```

hoặc nhiều columns:
```python
result = df.groupby("category").agg(
    total_sales=("sales", "sum"),
    avg_price=("price", "mean"),
    count=("id", "count")
)
```

Thông thường sau khi chạy xong, cột define trong `groupby` sẽ trở thành index, lúc này có thể sử dụng:
```python
result = result.reset_index()
```

để tách cột ra và trả về cột index bình thường ở đầu bảng.

---
# Merge / Join

Ví dụ mình có hai bảng là `customers` và `orders`, mình sẽ join bằng cách:
```python
result = pd.merge(
    orders,
    customers,
    on="customer_id",
    how="left"
)
```

nếu muốn join với nhiều key, sử dụng `on=["id", "date"]`. Nếu key có tên khác nhau ở hai bản có thể sử dụng:
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

Nếu muốn nối 2 rows với nhau, sử dụng syntax:
```python
df = pd.concat(
    [df1, df2],
    ignore_index=True
)
```

Nếu muốn nối 2 columns với nhau, sử dụng syntax:
```python
pd.concat(
    [df1, df2],
    axis=1
)
```

---
# Pivot Table

Pivot được sử dụng để có thể reshape va summarize data. Ví dụ như:
```python
pd.pivot_table(
    df,
    values="sales",
    index="category",
    columns="years",
    aggfunc="sum"
)
```

Ví dụ với bảng cụ thể:

| category | years | sales |
|----------|-------|-------|
| Áo       | 2023  | 100   |
| Áo       | 2023  | 150   |
| Áo       | 2024  | 200   |
| Quần     | 2023  | 300   |
| Quần     | 2024  | 400   |


sau khi chạy xong sẽ cho ra kết quả mới:

| category | 2023 | 2024 |
|----------|------|------|
| Áo       | 250  | 200  |
| Quần     | 300  | 400  |

Khi nhắc tới pivot, nó sẽ có hai kiểu đó là `pivot` và `pivot_table`. Với `df.pivot()` thì nó chỉ đơn thuần là xoay bảng không có tính toán, nếu dữ liệu gốc có 2 dòng trở lên trùng lặp, ví dụ như category hay years thì pivot nó sẽ văng lỗi. Với thằng còn lại nó sẽ an toàn hơn vì có `aggfunc`, khi gặp các dòng trùng nhau, nó sẽ tự động dùng hàm để gộp lại thành 1 con số duy nhất.

Ví dụ cụ thể:
```python
pd.pivot_table(
    df,
    values="sales",
    index="category",
    columns="years",
    aggfunc="sum",
    fill_value=0,   # Điền số 0 vào các ô bị khuyết (NaN) thay vì để trống
    margins=True    # Tự động thêm một cột total ở cuối (cộng dồn)
)
```
---
# Datetime

```python
df["date"] = pd.to_datetime(         # chuyển đổi kiểu dữ liệu
    df["date"]
)

df["year"] = df["date"].dt.year      # Lấy thông tin year, tương tự cho month, day, dayname
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

Với string column, mình sẽ sử dụng `.str`:
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
# trả về một array chứa các tên danh mục không trùng lặp
df["category"].unique()

# trả về 1 con số nguyên duy nhất (không đếm NaN), ví dụ trả về 2 nghĩa là có tổng 2 giá trị unique
df["category"].nunique()
```

Frequency:
```python
# Đếm xem mỗi giá trị xuất hiện bao nhiêu lần trong cột và tự động sắp xếp từ nhiều nhất đến ít nhất
# Biến normalize đổi số lượng đếm thành tỷ lệ phần trăm
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

| Mục đích           | Syntax                          |
|--------------------|---------------------------------|
| Import             | `import pandas as pd`           |
| Create DataFrame   | `pd.DataFrame(data)`            |
| Read CSV           | `pd.read_csv("file.csv")`       |
| Read Excel         | `pd.read_excel("file.xlsx")`    |
| Read JSON          | `pd.read_json("file.json")`     |
| Xem đầu            | `df.head()`                     |
| Xem cuối           | `df.tail()`                     |
| Thông tin          | `df.info()`                     |
| Thống kê           | `df.describe()`                 |
| Kích thước         | `df.shape`                      |
| Columns            | `df.columns`                    |
| Dtypes             | `df.dtypes`                     |
| Một column         | `df["col"]`                     |
| Nhiều columns      | `df[["a", "b"]]`                |
| Label selection    | `df.loc[...]`                   |
| Position selection | `df.iloc[...]`                  |
| Filter             | `df[df["age"] > 18]`            |
| AND                | `(cond1) & (cond2)`             |
| OR                 | `(cond1) \| (cond2)`            |
| Trong list         | `df["col"].isin([...])`         |
| Sort               | `df.sort_values("col")`         |
| Missing            | `df.isna()`                     |
| Đếm missing        | `df.isna().sum()`               |
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