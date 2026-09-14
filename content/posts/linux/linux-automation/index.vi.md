---
title: "Cron for Automation on Ubuntu"
date: 2026-08-24
draft: false
tags: ["Linux"]
description: "Cron và Automation trên Ubuntu"
---
# Cron

`cron` là scheduler dùng để tự động chạy command hoặc script theo lịch, ví dụ như backup, cleanup, log rotation, etc. Nó chạy như một background service và kiểm tra lịch theo từng phút

`Ubuntu` thường sẽ có sẵn `cron` nhưng có thể chủ động bằng cách:
```shell
sudo apt update
sudo apt install cron

sudo systemctl enable --now cron
```

Kiểm tra bằng cách chạy:
```shell
systemctl is-active cron
```

---
# Crontab

Mỗi user có một `crontab` riêng để lưu cron jobs.
```shell
crontab -e
```

---
# Cron Syntax

Cấu trúc: `minute hour day_of_month month day_of_week command`. Lưu ý:
```text
minute      : 0-59
hour        : 0-23
day         : 1-31
month       : 1-12
day of week : 0-7
```

> `0` và `7` đều là Sunday

---
# Syntax

```text
* = tất cả
, = list
- = range
/ = step
```

Ví dụ:
```text
* * * * * tương đương với mỗi phút

0,30 * * * * tương đương phút 0 và 30 mỗi giờ

0 9 * * 1-5 tương đương 9 giờ từ thứ hai tới thứ năm

*/15 * * * * tương đương mỗi 15 phút
```

---
# Special strings

Cron có hỗ trợ cách viết ngắn:
```text
@hourly
@daily
@weekly
@monthly
@yearly
@reboot
```

---
# Mange Crontab

Chỉnh sửa:
```shell
crontab -e
```

Xem:
```shell
crontab -l
```

Xóa:
```shell
crontab -r
```

Xóa nhưng có confirmation:
```shell
crontab -r -i
```

Với user khác:
```shell
sudo crontab -u username -l
```

---
# Environment

Cron không chạy với environment giống terminal của mình, nó thường có env tối giản như:
```text
SHELL=/bin/sh
PATH=/usr/bin:/bin
HOME=/home/user
```

Cũng chính vì vậy command chạy được trong terminal có thể fail trong cron. Cách an toàn là sử dụng absolute path:
```text
/usr/bin/python3
/home/user/script.sh
```

hoặc khai báo ở đầu crontab:
```text
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

---
# System-wide Cron

Ngoài crontab của từng user, còn có:
```text
/etc/cron.hourly/
/etc/cron.daily/
/etc/cron.weekly/
/etc/cron.monthly/
/etc/cron.d/
```

`/etc/cron.d/` dùng custom schedule và có thêm field `username`.

---
# Debug Cron

Xem cron jobs:
```shell
crontab -l
```

Xem cron service:
```text
systemctl status cron
```

Xem log:
```text
journalctl -u cron
```

Ví dụ có thể kiểm tra hôm nay:
```shell
journalctl -u cron --since today
```

---
# Cheatsheet

| Command / Syntax        | Ý nghĩa             |
|:------------------------|:--------------------|
| `cron`                  | scheduler           |
| `crontab`               | danh sách cron jobs |
| `crontab -e`            | edit jobs           |
| `crontab -l`            | list jobs           |
| `crontab -r`            | xóa crontab         |
| `crontab -r -i`         | xóa có confirmation |
| `systemctl status cron` | kiểm tra cron       |
| `journalctl -u cron`    | xem log             |
