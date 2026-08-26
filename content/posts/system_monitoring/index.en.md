---
title: "System Monitoring"
date: 2026-08-24
draft: false
tags: ["Linux"]
description: "Some basics information for system monitoring"
---
# ps - Process

A process is an instance of a program being executed, each command I run will usually create a process.
```shell
ps
```

`ps` gives a snapshot at the time it is run, unlike `top` which updates in realtime.

The normal output will include:
- `PID`: unique process id.
- `TTY`: terminal attached to the process.
- `TIME`: CPU time used by the process.
- `CMD`: command/program.

```shell
ps aux
```

`a`: processes of all users.
`u`: user-oriented/detailed format.
`x`: includes processes without a controlling terminal.

`top` displays processes in realtime, while `ps` is more suitable for query/scripting.

---
# Controlling terminal

A process can be attached to a controlling terminal - the terminal/session from which the process was started.

Example:
```shell
pts/0
pts/1
tty1
```

In `ps`, `TTY` indicates the terminal of the process.

---
# Process details

The linux kernel manages all processes and stores information. There are two important ids, `PID` and `PPID`, corresponding to process id and parent process id.

```shell
ps -ef
ps -l
```

A process usually belongs to a process tree, which can be viewed with:
```shell
pstree
```

---
# Process Creation

Linux mainly creates processes following the `fork -> exec` model.

`fork()`: The current process creates a child process that is almost a copy of itself. The child will have its own `PID` and its `PPID` will be the parent’s `PID`.

`exec()`: The child then usually calls `exec` to replace the current program with a new program.

---
# Process Termination

A process that terminates can return an exit status.
```shell
0     = success
non-0 = error/failure
```

In the shell, you can view the exit status of the previous command.
```shell
echo $?
```

---
# Signals

Signal is a mechanism used by the kernel/process to notify an event or request a process to perform an action

| Signal     | Number | Meaning                          |
|:-----------|:-------|:---------------------------------|
| `SIGHUP`   | 1      | common hangup/reload             |
| `SIGINT`   | 2      | interrupt, usually from `Ctrl+C` |
| `SIGKILL`  | 9      | Force the process to terminate   |
| `SIGTERM`  | 15     | Request graceful termination     |
| `SIGSTOP`  | 19*    | Force the process to stop        |
| `SIGCOUNT` | 18*    | Continue the process             |

`*Number` depends on the architecture, so signal names should be preferred.

List:
```shell
kill -l
```

Signal is the way the OS requests a process to terminate or change behavior.

---
# Kill - Terminate

`kill` is the command used to send a signal to a process. By default it sends `SIGTERM`, which is signal 15.
```shell
kill PID
```

Resources/files/state should be cleaned up first, `kill -9` should not be used by default.

`pkill` can be used to find/send signals by name or pattern and `killall` can send signals to processes with matching names.
```shell
pkill process_name
killall process_name
```

---
# Niceness

The Linux scheduler decides which process gets the CPU, and the nice value affects CPU scheduling priority. For example, `-20` is the highest priority, `0` is the default and `19` is the lowest priority.
```shell
nice -n 10 command
renice 10 -p PID
```

`nice` is used when starting a process, while `renice` is used for a process that is already running.

---
# Process States

A process is not always using the CPU, common states seen in `STAT`/`S` include:
- R = Running / runable - the process is running or ready to be run by the CPU scheduler.
- S = Interruptible sleep - the process is waiting for an event or resource and can be awakened by a signal.
- D = Uninterruptible sleep - usually related to I/O, the process is waiting for the kernel/resource and does not respond normally to signals at that time.
- T = Stopped / traced - the process is stopped.
- Z = Zombie - the child has terminated and the parent has not collected the exit status, meaning it no longer executes code like a normal process but still has an entry in the process table.

---
# /proc filesystem

`/proc` is a virtual filesystem that provides information about the kernel and processes, `/proc` is the filesystem containing process information.

Each process will usually have a dir:
```shell
/proc/<PID>/
ls /proc/1234
```

`proc` looks like files/dirs but most of the information is generated automatically by the kernel, not normal files stored on disk.

---
# Job Control

Job control is the shell’s ability to manage processes/jobs running in the foreground and background.

Foreground is a process that occupies the terminal and interacts directly with me, while background is a process running in the background, the shell will return the prompt to me.
```shell
ping google.com
ping google.com &
```

A foreground job can be stopped with `Ctrl+Z` and `bg` or `fg` can be used to decide how the job continues to run, for example:
```shell
fg %1
bg %1
```

---
# Cheatsheet

| Concept / Command | Meaning                                      |
|:------------------|:---------------------------------------------|
| process           | program being executed                       |
| `PID`             | Process ID                                   |
| `PPID`            | Parent Process ID                            |
| `ps`              | snapshot process                             |
| `ps aux`          | view detailed processes system-wide          |
| `ps -ef`          | view processes + PID/PPID in full format     |
| `top`             | monitor processes in realtime                |
| `TTY`             | controlling terminal                         |
| `?` in TTY        | no controlling terminal                      |
| `fork()`          | create a child process                       |
| `exec()`          | replace the process image with a new program |
| PID `1`           | root user-space process (`systemd`/`init`)   |
| `$?`              | exit status of the previous command          |
| `0`               | success                                      |
| `kill PID`        | send `SIGTERM`                               |
| `kill -9 PID`     | send `SIGKILL`                               |
| `kill -l`         | list signals                                 |
| `pkill`           | send signal by process name/pattern          |
| `killall`         | send signal by process name                  |
| `nice`            | set nice when starting                       |
| `renice`          | change nice of a running process             |
| `/proc/<PID>`     | process information from the kernel          |
| `jobs`            | view shell jobs                              |
| `&`               | run in background                            |
| `Ctrl+Z`          | suspend foreground job                       |
| `bg`              | continue in background                       |
| `fg`              | bring job to foreground                      |