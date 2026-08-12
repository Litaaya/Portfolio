---
title: "Linux Command Line"
date: 2026-08-12
draft: false
tags: ["Linux"]
description: "Một vài command cơ bản của linux"
---

> Ở phần này, chủ yếu mình sẽ tập trung vào bash shell

---

# Linux Command Line

## 1. Shell Basics

### Shell

Khi vào terminal của linux, sẽ có hai dạng lệnh có sẵn xuất hiện, một là đầu mục bắt đầu với `$` nghĩa là mình là normal user, còn `#` nghĩa là mình đang là root/superuser.

#### Syntax

```shell
command options arguments
```

#### Ví dụ

```shell
echo Hello World
```

Khi làm việc với terminal, một vài thao tác cơ bản sẽ bao gồm:

- `Enter` để thực thi lệnh
- `Up Arrow` để quay lại câu lệnh trước đó
- `Ctrl - C` để hủy lệnh nếu command bị stuck

---

## 2. Navigation

### pwd

Các file thư mục trong linux và đa số các hệ điều hành khác đều được tổ chức theo dạng tree hierarchy, có thể tưởng tượng như phân nhánh của cây vậy. Lệnh này giúp mình biết được mình đang đứng ở đâu.

#### Syntax

```shell
pwd
```

---

### cd

Đơn thuần là di chuyển tới chỗ khác thôi, có thể đi kèm với một vài node đặc biệt để chi tiết hơn.

- Với `.` nó sẽ đứng yên
- Với `..` nó sẽ lùi lại một bậc trên nhánh
- Với `~` thì nó sẽ quay ngược về thư mục home luôn
- Với `-` thì nó sẽ quay lại thư mục lần cuối mình đứng

#### Syntax

```shell
cd .
cd ..
cd ~
cd -
```

---

### ls

Liệt kê mọi thứ trong khu vực mình đang đứng hoặc có thể chỉ định một thư mục cố định.

Có thể đính kèm `-a` để coi hidden files, `-l` để coi thông tin chi tiết hơn của từng direct/thư mục hoặc `-lh` để dễ nhìn hơn, với `-r` là để thay đổi thứ tự sort > có thể kết hợp chung ví như `ls -ltr` để sort theo time và thay đổi thứ tự.

#### Syntax

```shell
ls
ls /home/litaaya
```

#### Options

```shell
-a
-l
-h
-r
-t
-S > sort theo size của file
-d */ > hiện thị mỗi cái tên thư mục hiện tại mình đang đứng
```

---

## 3. File Management

### touch

Mục đích đầu tiên là nó có thể tạo file mới với syntax:

```shell
touch [OPTIONS] FILE
```

Mục đích hai là nó sử dụng để update timestamp của file.

Mọi file trong linux sẽ có 3 mốc thời gian, bao gồm:

- atime là access time > thời điểm file được đọc/mở lần cuối
- mtime là Modification Time > thời điểm nội dung file bị chỉnh sửa lần cuối
- cuối cùng là ctime là Change Time > thời điểm thông tin/metadata của file bị sửa đổi.

#### Syntax

```shell
touch FILE > update lại timestamp của file
touch -r 1.txt 2.txt > copy timestamp từ file này sang file kia
touch -d "2026-08-12 00:00" file > set up date và time cố định cho file
touch -t 202608120000 file > set thời gian tương tự -d
touch -c FILE > update nếu file exist và bỏ qua, không create nếu nó không tồn tại
touch -a > chỉ cập nhật mốc thời gian của thời gian truy cập
touch -m > chỉ cập nhật mốc thời gian của thời gian chỉnh sửa nội dung
```

---

### cp

Copy files từ chỗ này qua chỗ kia.

#### Syntax

```shell
cp _ _ _ /home/litaaya/
cp _ /home/litaaya/_
cp *.py /home/... > tất cả các file với đuôi py
cp file? destination > tất cả file1, file2, filex, không file14
cp file[a-z] destination > fileb, filec, không file3
cp -r dir/ /home/litaaya/doc > copy toàn bộ trong thư mục dir và bỏ vào doc
cp -i > đề phòng overwrite, sử dụng để được thông báo trước
cp -f > force
cp -n > ngăn overwrite
cp -p > copy và giữ lại thông tin permission, ownership và timestamp của file gốc
cp -a > lấy sạch ko chừa cái gì
cp -u > chỉ copy new file
cp -v > show ra các file được copied
```

---

### mv

#### Đặt lại tên

```shell
mv old new
```

#### Di chuyển

Di chuyển, lưu ý nó sẽ overwrite nếu có file tồn tại sẵn, nên sử dụng `-i`/`n` hoặc `-b` (backup) hoặc `-v` để biết mv nó đang làm cái gì.

```shell
mv file1 file2 /dir
mv -t dir/ file1 file2
```

---

### mkdir

Tạo directory tại nơi mình đang đứng

#### Syntax

```shell
mkdir _ _
mkdir -p _/_/_ > ci này để tạo sâu hơn
mkdir -m > set up permission cho thư mục đó
mkdir -v > in ra message khi tạo thư mục đó
```

---

### rm

Nó đơn thuần là xóa _.

#### Syntax

```shell
rm _ _ _
rm -i _ > xem thông tin và xác nhận trước khi xóa
rm -I _ > prompt thông tin đúng duy nhất 1 lần
rm -f > force
rm -r > xóa đường dẫn dir
rmdir > cũng là xóa đường dẫn dir nhưng nó an toàn hơn vì dir phải empty nó mới cho xóa
rm -v > show ra cái gì sẽ bị xóa
```

---

## 4. Viewing Files

### file

`file` sẽ giúp mình coi được cái file này là cái gì, ví dụ như jpeg, txt, py, etc.

#### Syntax

```shell
file 1 2 3 4 > có thể check nhiều file
file -i _ > coi thông tin theo dạng MIME và charset theo chuẩn nhất định
file -b _ > cắt cái tên file ở đầu ra output
file -L link > cái này sẽ kết hợp với lối tắt sẽ được mô tả sau, ngắn gọn thì nó sẽ xem cái link trỏ tới đâu và file đó là gì
file -z file.txt.gz > thử xem coi bên trong cái compressed file này là gì
```

---

### cat

`cat` khi được sử dụng với `>` sẽ tạo file mới, lưu ý nếu xài khi file exist sẽ hoàn toàn ghi đè lên file sẵn có, nếu muốn append hãy sử dụng `>>`.

Trong quá trình sử dụng, có thể `Ctrl - -D` khi đang ở new line để save và exit.

#### Syntax

```shell
cat file > in toàn bộ thông tin file ra màn hình, không khuyến nghị lắm
cat file1 file2 > file3 -> cái này sẽ in 1, in 2 nếu ko có dấu > và nếu có hì nó sẽ gộp 1 2 lại thành 3
cat -n file > đặt số cho output line
cat -b file > đặt số cho output không empty line
cat -s file > ép mấy cái dòng trống lại thành 1 dòng trống duy nhất
cat -A file > nó show hết mọi thứ ra, ví dụ trong file có ^I nghĩa là có Tab ở đó hay $ nghĩa là Enter
```

---

### less

Truy cập vào giao diện mới để xem file, trong đó mình có thể sử dụng arrow keys lên xuống để coi line by line hoặc `g` để đi lên đầu file, `G` để cuối file và `u` để lên, `d` để xuống, `h` để coi các command có thể sử dụng.

#### Syntax

```shell
/search_term > để tìm word từ trên xuống
?search_term > để tìm word từ dưới lên
n để qua từ kế và N để ngược lại từ trước
q > exit
less -N file > show số dòng
less +G file > mở ra ở dòng cuối của file
less +F > show ra log theo thời gian thực, có thể ctrl C để dừng trạng thái cập nhật theo thời gian thực
```

---

## 5. Command History

### history

Coi lịch sử các command mình đã thực hiện.

Với re running thì ngoài xài mũi tên lên, có thể sử dụng:

- `!!` để tự động chạy lại command gần nhất
- `!{number}` để chạy command số `{number}`
- `!cat` để chạy command gần nhất bắt đầu với `cat`

Để search trong lịch sử, có thể sử dụng `Ctrl R`, `Enter` để sử dụng nó.

#### Syntax

```shell
history -c > remove
history -w > lưu lại history
history -d <offset> > xóa cái command bởi cái số lịch sử của nó
```

---

## 6. Search

### find

Tìm kiếm cái mình cần.

#### Syntax

```shell
find dir -name _ > tìm _ ở trong dir và subdir
find dir -iname _ > bỏ qua case
find . -name "*.txt" > tìm tài liệu txt
find dir -type d -name _ > vô trong home, tìm dir có name là _ > nếu xài cho file thì sử dụng -type f
find . -type f -size +10M
find . -type f -size -1k
find . -type f -mtime -7 > trong vòng 7 ngày, còn nếu là + thì nghĩa là hơn 7 ngày
```

`find` có thể đi chung với `-print`, `-delete` hoặc `-exec`.

---

## 7. Getting Help

### help

Mình quên command, mình sử dụng nó.

#### Syntax

```shell
help _ > cái này là gì, sử dụng cho các build ins
_ --help > sử dụng cho các external binaries
man _ > mở manual page chi tiết của command
whatis _ > in ra 1 dòng mô tả ngắn gọn chức năng của command
type _ > để biết cái command này nên xài cái help hay --help
```

---

### man

Mở manual page, có thể sử dụng:

- `/` để search
- `q` để quit
- `n` và `N` để di chuyển giữa các thông tin

Man pages section có:

- `1` là user commands
- `2` là system calls
- `3` là library functions
- `5` là file formats
- `8` là system administration commands

---

## 8. Shell Customization

### alias

Tạo một cái alias tạm thời.

#### Syntax

```shell
alias ll='ls -la' > sử dụng ll tương đương sử dụng ls -la
unalias ll > xóa ll
alias > list ra các alias hiện tại
type ll > để biết ll làm cái gì
```

Để tạo permanent thì cần truy cập:

```shell
nano ~/.bashrc
```

Và viết vào đó:

```shell
alias ll='ls -la'
```

---

## 9. Exit

### exit

Như cái tên gọi, nó giúp mình thoát.

#### Syntax

```shell
exit
logout
Ctrl - D
```