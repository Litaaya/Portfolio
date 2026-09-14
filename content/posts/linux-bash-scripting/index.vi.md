---
title: "Linux Bash Scripting"
date: 2026-08-24
draft: false
tags: ["Linux"]
description: "Bash Scripting trong Linux"
---
# Linux Bash Utilities

Có thể sử dụng:
```shell
man command
info command
```

để tra cứu manual/info của command.

Nói riêng tới compression, nó dùng để giảm kích thước file với các tool phổ biến như `gzip`, `bzip2`, `xz`. Có thể giải nén bằng cách thêm `-d` vào command, ví dụ:
```shell
gzip -d file.txt.gzip
```

---
# Archive - zip và tar

`Archive` dùng để gom nhiều file/dir thành 1 file.

`zip`
```shell
zip archive.zip file1 file2
unzip archive.zip
```

`tar`
```shell
tar -cf archive.tar folder/
tar -xf archive.tar
```

Các option chính:
```shell
-c = create
-x = extract
-f = filename
```

---
# tar + Compression

Có thể vừa archive vừa compress bằng cách sử dụng:
```shell
tar -czf archive.tar.gz folder/
tar -xzf archive.tar.gz
```

Các option:
```shell
-z = gzip
-j = bzip2
-J = xz
```

---
# Network Utilities

`ping`: kiểm tra host có reachable hay không, command này thường dùng để kiểm tra connectivity.
```shell
ping google.com
```

`host`: DNS lookup, lệnh này giúp resolve domain -> IP address.
```shell
host google.com
```

`ipconfig`: xem thông tin network interface, mình có thể thấy IP và interface.
```shell
ipconfig
```

---
# wget và curl

Hai command này được sử dụng để tải dữ liệu qua network.
```shell
wget URL
curl -O URL
```

`wget` phù hợp để download file, `curl` linh hoạt hơn và đặc biệt với HTTP/API, có thể hình dung nó như một cách để transfer dữ liệu.

---
# Bash Script

`Bash Script` là file chứa một tập hợp các `Bash Command` để chạy tự động theo thứ tự.
```shell
#!/bin/bash

echo "Litaaya"
pwd
ls
```

Thay vì gõ từng command một, có thể gom thành `script.sh` để automation.

Ở trong một bash script, dòng đầu là `shebang`, nó cho hệ điều hành biết script sử dụng Bash interpreter. Có thể execute:
```shell
chmod +x script.sh
./script.sh
```

---
# Variables

Tạo:
```shell
name="Litaaya"
```

Lấy:
```shell
echo $name
```

Gán/Nhận input:
```shell
read name
```

---
# Script Arguments

Có thể truyền dữ liệu khi gọi script:
```shell
./script.sh hello world
```

Ví dụ trong script là:
```shell
echo $1
echo $2
```

thì kết quả là:
```shell
$1 = hello
$2 = world
```

---
# Conditionals

Conditional giúp script ra quyết định:
```shell
if [condition]; then
  command
elif [condition]; then
  command
else [condition];
  command
fi
```

Ví dụ:
```shell
age=23

if [$age -ge 25]; then
  echo "Cece"
else
  echo "Sage"
```

Các comparison:
```shell
# Numeric
-eq = equal
-ne = not equal
-gt = greater than
-ge = greater than or equal
-lt = less than
-le = less than or equal

# String
== = equal
!= = not equal
```

---
# Loops

Loop dùng để chạy command nhiều lần.

`for`
```shell
for file in *.txt
do
  echo $file
done
```

`while`
```shell
count=1

while [$count -le 5]
do
  echo $count
  count=$((count + 1))
done
```

---
# Cheatsheet

| Command / Syntax | Ý nghĩa                   |
|:-----------------|:--------------------------|
| `man command`    | xem manual                |
| `info command`   | xem documentation         |
| `gzip`           | nén `.gz`                 |
| `bzip2`          | nén `.bz2`                |
| `xz`             | nén `.xz`                 |
| `-d`             | decompress                |
| `zip`            | archive + compression     |
| `unzip`          | extract `.zip`            |
| `tar -cf`        | create archive            |
| `tar -xf`        | extract archive           |
| `tar -czf`       | tar + gzip                |
| `tar -cjf`       | tar + bzip2               |
| `tar -cJf`       | tar + xz                  |
| `ping`           | kiểm tra connectivity     |
| `host`           | DNS lookup                |
| `ifconfig`       | xem network interface     |
| `wget`           | download file             |
| `curl`           | transfer/download dữ liệu |
| `#!/bin/bash`    | Bash shebang              |
| `var=value`      | tạo variable              |
| `$var`           | lấy giá trị variable      |
| `read var`       | nhận user input           |
| `$1`, `$2`       | script arguments          |
| `if ... fi`      | conditional               |
| `for ... done`   | loop                      |
| `while ... done` | conditional loop          |
| `alias`          | tạo alias                 |