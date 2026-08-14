---
title: "Linux Permissions"
date: 2026-08-14
draft: false
tags: ["Linux"]
description: "Permissions in Linux"
---

# File permissions

Linux controls access permissions based on 3 user groups:
- `u` - user/owner: the owner
- `g` - group: the owning group
- `o` - others: everyone else

Each group has 3 basic permissions:
- `r` - read: read file contents, view the list of files in a dir
- `w` - write: modify file contents, create/delete/rename entries in a dir
- `x` - execute: run a file, enter a dir

Example:
```shell
drwxr-xr-x > d is the file type, wxr means the owner's permissions, xr belongs to the group and x belongs to others
```

---
# Modifying permissions

`chmod` is used to change permissions.

Symbolic mode:
```shell
chmod [u/g/o/a][+/-/=][r/w/x] file
```
```shell
u = user
g = group
o = others
a = all

+ = add permission
- = remove permission
= = set the exact permission
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

Besides permissions, each file also has an owner and a group.

In the command below, litaaya is the owner of the file and developers is the group:
```shell
ls -l
-rw-r--r-- 1 litaaya developers 1024 file.txt
```

`chown` is used to change the owner, for example:
```shell
sudo chown cece file.txt
sudo chown cece:devops file.txt
```

`chgrp` is used to change the group, for example:
```shell
chgrp devops file.txt
```

Recursive can be used to change the information of all subdirectories inside a dir.
```shell
chown -R cece:devops folder/
```

---
# Umask

`umask` determines the default permissions when a new file or dir is created.

Example:
```shell
file : 666 = rw-rw-rw-
dir : 777 = rwxrwxrwx

umask = 022

file : 644 - rw-r--r--
dir : 755 - rwxr-xr-x
```

---
# Setuid

When I run a program, the process runs with the permissions of the user running the program, `setuid` changes this.

Example:
```shell
chmod u+s program
chmod 4755 program
```

In the numeric case, `suid` = 4.

`Suid` allows a user to temporarily perform an action with the permissions of the program owner.

---
# Setgid

`Setgid` is similar to `Setuid` but is related to the group.

`Setgid` = 2.

Example:
```shell
chmod g+s shared/
chmod 2755 shared/
```

---
# Process permissions

A process in linux does not have only a single `uid`.

## Real UID - RUID

> Indicates which user started the process.

## Effective UID - EUID

> Indicates under whose permissions the process is currently being executed.

## SUID

`Suid` can make `euid` different from `ruid`.

---
# The sticky bit

```shell
drwxrwxrwt
```

`Sticky bit` restricts deleting/renaming files in a shared dir; normally only the file owner, the dir owner, or a privileged user is allowed to perform that operation.

Example:
```shell
chmod +t dir
chmod 1777 dir
```

Because `sticky` = 1

---
# Cheatsheet

> Too much to remember, so I created this section to search more quickly.

| Concept      | Meaning                                         |
|:-------------|:------------------------------------------------|
| `r`          | read                                            |
| `w`          | write                                           |
| `x`          | execute                                         |
| `u`          | owner/user                                      |
| `g`          | group                                           |
| `o`          | others                                          |
| `a`          | all                                             |
| `chmod`      | change permissions                              |
| `chown`      | change owner/group                              |
| `chgrp`      | change group                                    |
| `umask`      | limit default permissions                       |
| `suid`       | run with the owner's effective user             |
| `sgid`       | run with effective group/inherit group on dir   |
| `sticky bit` | restrict deleting files in a shared dir         |

Numeric:
```shell
r = 4
w = 2
x = 1

suid = 4xxx
sgid = 2xxx
sticky = 1xxx
```