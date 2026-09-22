---
title: "Pandas"
date: 2026-09-21
draft: true
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