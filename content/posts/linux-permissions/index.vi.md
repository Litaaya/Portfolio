---
title: "Linux Permissions"
date: 2026-08-14
draft: false
tags: ["Linux"]
description: "Permissions in Linux"
---

# File permissions

Linux sẽ kiểm soát quyền truy cập theo 3 nhóm user:
- `u` - user/owner: người sở hữu
- `g` - group: nhóm sở hữu
- `o` - others: tất cả người còn lại

Mỗi nhóm có 3 quyền cơ bản:
- `r` - read: đọc nội dung file, xem danh sách file trong dir
- `w` - write: sửa nội dung file, tạo/xóa/đổi tên entry trong dir
- `x` - execute: chạy file, đi vào dir

Ví dụ:
```shell
drwxr-xr-x > d là file type, wxr nghĩa là quyền owner, xr là của group và x là của others
```

---
# Modifying permissions

`chmod` được sử dụng để thay đổi permission.

Symbolic mode:
```shell
chmod [u/g/o/a][+/-/=][r/w/x] file
```
```shell
u = user
g = group
o = others
a = all

+ = thêm quyền
- = bỏ quyền
= = đặt chính xác quyền
```
Numeric mode:
```shell
chmod [num][num][num] file
```
```shell
r = 4
w = 2
x = 1
```

---
# Ownership permission

Ngoài permission, mỗi file còn có owner và group.

Ví dụ ở câu lệnh dưới, litaaya là owner của file và developers là group:
```shell
ls -l
-rw-r--r-- 1 litaaya developers 1024 file.txt
```

`chown` được sử dụng để thay owner, ví dụ:
```shell
sudo chown cece file.txt
sudo chown cece:devops file.txt
```

`chgrp` được sử dụng để thay group, ví dụ:
```shell
chgrp devops file.txt
```

Recursive có thể được sử dụng để thay đổi thông tin của toàn bộ các thư mục con trong một dir.
```shell
chown -R cece:devops folder/
```

---
# Umask

`umask` quyết định permission mặc định khi một file hoặc dir mới được tạo.

Ví dụ:
```shell
file : 666 = rw-rw-rw-
dir : 777 = rwxrwxrwx

umask = 022

file : 644 - rw-r--r--
dir : 755 - rwxr-xr-x
```

---
# Setuid

Khi mình chạy một program, process chạy với quyền của user đang chạy chương trình, `setuid` sẽ thay đổi cái này.

Ví dụ:
```shell
chmod u+s program
chmod 4755 program
```

Với trường hợp numeric, `suid` = 4.

`Suid` cho phép user tạm thời thực hiện một hành động bằng quyền owner chương trình.

---
# Setgid

`Setgid` tương tự với `Setuid` nhưng liên quan đến group.

`Setgid` = 2.

Ví dụ:
```shell
chmod g+s shared/
chmod 2755 shared/
```

---
# Process permissions

Process trong linux không chỉ có một `uid` duy nhất.

## Real UID - RUID

> Cho biết user nào khởi chạy process.

## Effective UID - EUID

> Cho biết process hiện đang được thực thi dưới quyền của ai.

## SUID

`Suid` có thể làm `euid` khác `ruid`.

---
# The sticky bit

```shell
drwxrwxrwt
```

`Sticky bit` hạn chế việc xóa/rename file trong dir dùng chung, thông thường chỉ owner của file, owner của dir hoặc privileged user mới được phép thực hiện thao tác đó.

Ví dụ:
```shell
chmod +t dir
chmod 1777 dir
```

Vì `sticky` = 1

---
# Cheatsheet

> Không nhớ nổi, nhiều quá nên tạo mốt search cho nhanh.

| Khái niệm    | Ý nghĩa                                         |
|:-------------|:------------------------------------------------|
| `r`          | read                                            |
| `w`          | write                                           |
| `x`          | execute                                         |
| `u`          | owner/user                                      |
| `g`          | group                                           |
| `o`          | others                                          |
| `a`          | all                                             |
| `chmod`      | thay permission                                 |
| `chown`      | thay owner/group                                |
| `chgrp`      | thay group                                      |
| `umask`      | giới hạn permission mặc định                    |
| `suid`       | chạy với effective user của owner               |
| `sgid`       | chạy với effective group/inherit group trên dir |
| `sticky bit` | hạn chế xóa file trong shared dir               |

Numeric:
```shell
r = 4
w = 2
x = 1

suid = 4xxx
sgid = 2xxx
sticky = 1xxx
```