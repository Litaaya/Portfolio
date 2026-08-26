---
title: "System Monitoring"
date: 2026-08-24
draft: false
tags: ["Linux"]
description: "Một vài kiến thức cơ bản để giám sát hệ thống"
---
# ps - Process

Một process là một instance của chương trình đang được thực thi, mỗi command mình chạy thường sẽ tạo ra một process.
```shell
ps
```

`ps` cho một snapshot tại thời điểm chạy, khác với `top` cập nhật realtime.

Output thông thường sẽ bao gồm:
- `PID`: process id duy nhất.
- `TTY`: terminal gắn với process.
- `TIME`: CPU time process đã dùng.
- `CMD`: command/program.

```shell
ps aux
```

`a`: processes của các users.
`u`: user-oriented/detailed format.
`x`: kể cả process không có controlling terminal.

`top` hiển thị process theo thời gian thực, còn `ps` thích hợp hơn cho việc query/scripting.

---
# Controlling terminal

Một process có thể gắn với một controlling terminal - terminal/session mà process được khởi chạy từ đó.

Ví dụ:
```shell
pts/0
pts/1
tty1
```

Trong `ps` thì `TTY` cho biết terminal của process.

---
# Process details

Kernel linux quản lý toàn bộ process và lưu thông tin. Có hai id quan trọng là `PID` và `PPID` tương ứng là process id và parent process id.

```shell
ps -ef
ps -l
```

Một process thường thuộc một process tree, có thể xem bằng:
```shell
pstree
```

---
# Process Creation

Linux chủ yếu tạo process theo mô hình `fork -> exec`.

`fork()`: Process hiện tại tạo một child process gần như là bản sao của nó. Child sẽ có `PID` riêng và `PPID` là `PID` của parent.

`exec()`: Sau đó child thường gọi `exec` để thay chương trình hiện tại bằng một chương trình mới.

---
# Process Termination

Một process kết thúc có thể trả về exit status.
```shell
0     = success
non-0 = error/failure
```

Trong shell có thể coi exit status của command trước.
```shell
echo $?
```

---
# Signals

Signal là cơ chế kernel/process dùng để thông báo một event hoặc yêu cầu process thực hiện hành động

| Signal     | Number | Ý nghĩa                       |
|:-----------|:-------|:------------------------------|
| `SIGHUP`   | 1      | hangup/reload thường gặp      |
| `SIGINT`   | 2      | interrupt, thường từ `Ctrl+C` |
| `SIGKILL`  | 9      | Buộc process kết thúc         |
| `SIGTERM`  | 15     | Yêu cầu kết thúc graceful     |
| `SIGSTOP`  | 19*    | Buộc process dừng             |
| `SIGCOUNT` | 18*    | Tiếp tục process              |

`*Number` phụ thuộc vào architecture, nên sẽ ưu tiên signal name.

Liệt kê:
```shell
kill -l
```

Signal là cách OS yêu cầu process terminate hoặc thay đổi hành vi.

---
# Kill - Terminate

`kill` là command gửi signal cho process. Mặc định sẽ là gửi `SIGTERM` tức là signal 15.
```shell
kill PID
```

Nên cleanup resource/file/state trước, không nên mặc định `kill -9`.

Có thể sử dụng `pkill` để tìm/gửi signal theo tên hoặc pattern và `killall` có thể gửi signal cho các process có tên tương ứng.
```shell
pkill process_name
killall process_name
```

---
# Niceness

Linux scheduler quyết định process nào được CPU, nice value sẽ ảnh hưởng CPU scheduling priority. Ví dụ với `-20` thì sẽ là highest priority, `0` là default và `19` là lowest priority.
```shell
nice -n 10 command
renice 10 -p PID
```

`nice` dùng lúc bắt đầu process, còn `renice` dùng cho process đã chạy.

---
# Process States

Process không phải lúc nào cũng đang dùng CPU, các trạng thái thường thấy trong `STAT`/`S` bao gồm:
- R = Running / runable - proces đang chạy hoặc sẵn sàng được CPU scheduler chạy.
- S = Interruptible sleep - process đang chờ một event hoặc resource và có thể bị đánh thức bởi signal.
- D = Uninterruptible sleep - thường liên quan tới I/O, process đang chờ kernel/resource và không phản ứng bình thường với signal lúc đó.
- T = Stopped / traced - process đang stop.
- Z = Zombie - child đã terminate và parent chưa collect được exit status, nghĩa là không còn thực thi code như process bình thường nhưng vẫn còn entry trong process table.

---
# /proc filesystem

`/proc` là virtual filesystem cung cấp thông tin về kernel và process, `/proc` là filesystem chứa process information.

Mỗi process sẽ thường có dir là:
```shell
/proc/<PID>/
ls /proc/1234
```

`proc` nhìn như file/dir nhưng phần lớn thông tin được kernel tạo tự động, không phải file thông thường nằm trên disk.

---
# Job Control

Job control là khả năng shell quản lý các process/job chạy foreground và background.

Foreground là process chiếm terminal và tương tác trực tiếp vời mình, còn background là process chạy ngầm, shell sẽ trả lại prompt cho mình.
```shell
ping google.com
ping google.com &
```

Có thể stop foreground job bằng `Ctrl+Z` và `bg` hoặc `fg` để quyết định job chạy tiếp như thế nào, ví dụ:
```shell
fg %1
bg %1
```

---
# Cheatsheet

| Khái niệm / Command | Ý nghĩa                                   |
|:--------------------|:------------------------------------------|
| process             | program đang thực thi                     |
| `PID`               | Process ID                                |
| `PPID`              | Parent Process ID                         |
| `ps`                | snapshot process                          |
| `ps aux`            | xem process chi tiết toàn hệ thống        |
| `ps -ef`            | xem process + PID/PPID dạng full          |
| `top`               | monitor process realtime                  |
| `TTY`               | controlling terminal                      |
| `?` ở TTY           | không có controlling terminal             |
| `fork()`            | tạo child process                         |
| `exec()`            | thay process image bằng program mới       |
| PID `1`             | process gốc user-space (`systemd`/`init`) |
| `$?`                | exit status command trước                 |
| `0`                 | success                                   |
| `kill PID`          | gửi `SIGTERM`                             |
| `kill -9 PID`       | gửi `SIGKILL`                             |
| `kill -l`           | list signals                              |
| `pkill`             | gửi signal theo process name/pattern      |
| `killall`           | gửi signal theo tên process               |
| `nice`              | đặt nice khi khởi chạy                    |
| `renice`            | đổi nice của process đang chạy            |
| `/proc/<PID>`       | thông tin process từ kernel               |
| `jobs`              | xem jobs của shell                        |
| `&`                 | chạy background                           |
| `Ctrl+Z`            | suspend foreground job                    |
| `bg`                | tiếp tục ở background                     |
| `fg`                | đưa job về foreground                     |