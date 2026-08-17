---
title: "Linux Text Processing"
date: 2026-08-17
draft: false
tags: ["Linux"]
description: "Kiến thức cơ bản về text processing"
---
---
# stdout - standard output

`stdout` là output thông thường của một process, mặc định hiển thị trên terminal.
```shell
echo "hello"
```

Redirect output sang file
```shell
echo "hello" > file.txt
echo "hello" >> file.txt
```

`>`: ghi output vào file, overwrite nội dung cũ.

`>>`: append vào cuối file.

---
# stdin - standard input

`stdin` là input mặc định của process.

```shell
cat < file.txt
```

Flow: `keyboard/file` -> `stdin` -> `command`.

---
# stderr - standard error

`stderr` là stream dùng cho error messages, tách riêng khỏi `stdout`.

Linux có các file descriptor cơ bản như:
```shell
0 = stdin
1 = stdout
2 = stderr
```

Ví dụ:
```shell
command 2>> error.txt
command 1> output.txt 2>&1
```

---
# pipe and tee

`pipe` đưa `stdout` của command trước thành `stdin` của command sau.
```shell
command1 | command2
ls -l | grep ".txt"
```

`tee` sẽ đưa output ra terminal và vừa ghi output vào file.
```shell
ls | tee file.txt
ls | tee -a file.txt
```

---
# env - environment

Environment variables chứa các giá trị mà shell hoặc process có thể sử dụng.
```shell
env

echo $HOME
echo $USER
echo $PATH

my_var="hello"
export my_var="hello"
```

---
# cut

`cut` dùng để trích một phần của mỗi dòng text.

Theo character, ví dụ lấy ký tự đầu tiên mỗi dòng:
```shell
cut -c 1 file.txt
```

Khoảng ký tự:
```shell
cut -c 1-5 file.txt
```

Theo field:
```shell
cut -d ',' -f 2 data.csv
```

`-d`: delimeter.

`-f`: field.

`-c`: character.

---
# paste

`paste` ghép các dòng tương ứng của nhiều file với nhau. Mặc định seperator là `Tab`. Ở ví dụ dưới, ví dụ `-d` là xài delimeter còn `-s` là ghép các dòng của một file thành một dòng.
```shell
paste -d ',' file1.txt file2.txt
paste -s file.txt
```

---
# head và tail

`head`/`tail` tương ứng là xem phần đầu/cuối của file. Mặc định là 10 dòng.
```shell
head file.txt
head -n 5 file.txt
head -5 file.txt

tail file.txt
tail -n 5 file.txt
tail -5 file.txt
```

Với `tail` thì có thể sử dụng:
```shell
tail -f app.log
```

`-f` có thể sử dụng để theo dõi file và các dòng mới khi chúng được ghi vào.

---
# expand và unexpand

`expand` chuyển đổi `Tab` qua `Spaces` và `unexpand` thì ngược lại.

Có thể điều chỉnh `Tab` size:
```shell
expand -t 4 file.txt
```

---
# join và split

```shell
join file1.txt file2.txt
```

Nó tương tự `JOIN` bên sql, nhưng đơn giản hơn nhiều và cần dữ liệu đã được tổ chức phù hợp sẵn.
```shell
split file.txt
split -l 1000 bigfile.txt
```

Chia thành mỗi file 1000 dòng

---
# sort

`sort` dùng để sắp xếp các dòng text.
```shell
sort file.txt
sort -r file.txt
sort -n file.txt
sort -u file.txt
```

`-r`: reverse.
`-n`: numeric.
`-u`: unique.

---
# tr - translate

`tr` được sử dụng để thay đổi hoặc loại bỏ characters từ input stream
```shell
echo "hello" | tr 'a-z' 'A-Z'
```
sẽ cho ra kết quả là `HELLO`.

```shell
echo "a,b,c" | tr ',' ' '
```
sẽ cho ra kết quả là `a b c`.

```shell
echo "hello123" | tr -d '0-9'
```
sẽ cho ra kết quả là `hello`.

---
# uniq - unique

`uniq` loại bỏ các dòng duplicate liền kề nhau

> `uniq` chỉ xử lý các duplicate adjacent nên thường sẽ cần kết hợp với `sort`.
```shell
sort file.txt | uniq
sort file.txt | uniq -c
sort file.txt | uniq -d
```

`-c`: count.
`-d`: duplicates > hiện mỗi dup.

---
# wc và nl

`wc`: word count, dùng để đếm nội dung. Có thể xài chung với `-l` là số dòng, `-w` là số từ, `-c` là số byte.
```shell
wc -l file.txt
wc -w file.txt
wc -c file.txt
```

`nl` là thêm line numbers cho file text.
```shell
nl file.txt
```

---
# grep

`grep` được sử dụng để tìm một dòng khớp với một pattern.
```shell
grep "error" app.log
grep -i "error" app.log
grep -n "error" app.log
grep -v "DEBUG" app.log
grep -r "password"

ps aux | grep python
```

`-i`: bỏ qua case.
`-n`: line number.
`-v`: invert.
`-r`: recursive.

---
# Cheatsheet

| Command/symbol  | Ý nghĩa                                   |
|:----------------|:------------------------------------------|
| `stdout`        | standard output                           |
| `stdin`         | standard input                            |
| `stderr`        | standard error                            |
| `>`             | stdout -> file, overwrite                 |
| `>>`            | stdout -> file, append                    |
| `<`             | file -> stdin                             |
| `2>`            | stderr -> file                            |
| `2>>`           | append stderr                             |
| `\|`            | stdout command trước -> stdin command sau |
| `tee`           | output ra terminal và file                |
| `env`           | xem environment variables                 |
| `$VAR`          | lấy giá trị environment variable          |
| `export`        | đưa variable vào environment              |
| `cut`           | lấy characters/fields                     |
| `paste`         | ghép các dòng tương ứng                   |
| `head`          | xem đầu file                              |
| `tail`          | xem cuối file                             |
| `tail -f`       | theo dõi file realtime                    |
| `expand`        | tabs -> spaces                            |
| `unexpand`      | spaces -> tabs                            |
| `join`          | join hai file theo field                  |
| `split`         | chia file thành nhiều phần                |
| `sort`          | sắp xếp dòng                              |
| `tr`            | thay đổi/delete characters                |
| `uniq`          | bỏ adjacent duplicate lines               |
| `wc`            | đếm lines/words/bytes                     |
| `nl`            | đánh số dòng                              |
| `grep`          | tìm/filter theo pattern                   |


```shell
0 = stdin
1 = stdout
2 = stderr
```