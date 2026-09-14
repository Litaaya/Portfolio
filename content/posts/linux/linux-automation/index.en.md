---
title: "Cron for Automation on Ubuntu"
date: 2026-08-24
draft: false
tags: ["Linux"]
description: "Cron and Automation on Ubuntu"
---
# Cron

`cron` is a scheduler used to automatically run commands or scripts on a schedule, for example backup, cleanup, log rotation, etc. It runs as a background service and checks the schedule every minute

`Ubuntu` usually has `cron` available, but it can be installed manually by:
```shell
sudo apt update
sudo apt install cron

sudo systemctl enable --now cron
```

Check by running:
```shell
systemctl is-active cron
```

---
# Crontab

Each user has a separate `crontab` to store cron jobs.
```shell
crontab -e
```

---
# Cron Syntax

Structure: `minute hour day_of_month month day_of_week command`. Note:
```text
minute      : 0-59
hour        : 0-23
day         : 1-31
month       : 1-12
day of week : 0-7
```

> `0` and `7` are both Sunday

---
# Syntax

```text
* = all
, = list
- = range
/ = step
```

Example:
```text
* * * * * is equivalent to every minute

0,30 * * * * is equivalent to minute 0 and 30 every hour

0 9 * * 1-5 is equivalent to 9 o’clock from Monday to Thursday

*/15 * * * * is equivalent to every 15 minutes
```

---
# Special strings

Cron supports shorthand notation:
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

Edit:
```shell
crontab -e
```

View:
```shell
crontab -l
```

Delete:
```shell
crontab -r
```

Delete but with confirmation:
```shell
crontab -r -i
```

With another user:
```shell
sudo crontab -u username -l
```

---
# Environment

Cron does not run with the same environment as my terminal, it usually has a minimal env such as:
```text
SHELL=/bin/sh
PATH=/usr/bin:/bin
HOME=/home/user
```

This is also why a command that runs in the terminal can fail in cron. The safe way is to use an absolute path:
```text
/usr/bin/python3
/home/user/script.sh
```

or declare it at the beginning of the crontab:
```text
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

---
# System-wide Cron

In addition to each user’s crontab, there are also:
```text
/etc/cron.hourly/
/etc/cron.daily/
/etc/cron.weekly/
/etc/cron.monthly/
/etc/cron.d/
```

`/etc/cron.d/` is used for custom schedules and has an additional `username` field.

---
# Debug Cron

View cron jobs:
```shell
crontab -l
```

View cron service:
```text
systemctl status cron
```

View log:
```text
journalctl -u cron
```

For example, today can be checked with:
```shell
journalctl -u cron --since today
```

---
# Cheatsheet

| Command / Syntax        | Meaning                  |
|:------------------------|:-------------------------|
| `cron`                  | scheduler                |
| `crontab`               | list of cron jobs        |
| `crontab -e`            | edit jobs                |
| `crontab -l`            | list jobs                |
| `crontab -r`            | delete crontab           |
| `crontab -r -i`         | delete with confirmation |
| `systemctl status cron` | check cron               |
| `journalctl -u cron`    | view log                 |