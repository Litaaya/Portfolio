---
title: "Linux Bash Scripting"
date: 2026-08-24
draft: false
tags: ["Linux"]
description: "Bash Scripting in Linux"
---
# Linux Bash Utilities

Can use:
```shell
man command
info command
```

to look up the manual/info of the command.

Specifically regarding compression, it is used to reduce file size with common tools such as `gzip`, `bzip2`, `xz`. It can be decompressed by adding `-d` to the command, for example:
```shell
gzip -d file.txt.gzip
```

---
# Archive - zip and tar

`Archive` is used to combine multiple files/dirs into 1 file.

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

The main options:
```shell
-c = create
-x = extract
-f = filename
```

---
# tar + Compression

Can both archive and compress by using:
```shell
tar -czf archive.tar.gz folder/
tar -xzf archive.tar.gz
```

The options:
```shell
-z = gzip
-j = bzip2
-J = xz
```

---
# Network Utilities

`ping`: checks whether the host is reachable or not, this command is commonly used to check connectivity.
```shell
ping google.com
```

`host`: DNS lookup, this command helps resolve domain -> IP address.
```shell
host google.com
```

`ipconfig`: view network interface information, I can see the IP and interface.
```shell
ipconfig
```

---
# wget and curl

These two commands are used to download data over the network.
```shell
wget URL
curl -O URL
```

`wget` is suitable for downloading files, `curl` is more flexible and especially with HTTP/API, it can be thought of as a way to transfer data.

---
# Bash Script

`Bash Script` is a file containing a set of `Bash Command` to run automatically in order.
```shell
#!/bin/bash

echo "Litaaya"
pwd
ls
```

Instead of typing each command one by one, they can be grouped into `script.sh` for automation.

In a bash script, the first line is `shebang`, it tells the operating system that the script uses the Bash interpreter. It can be executed:
```shell
chmod +x script.sh
./script.sh
```

---
# Variables

Create:
```shell
name="Litaaya"
```

Get:
```shell
echo $name
```

Assign/Receive input:
```shell
read name
```

---
# Script Arguments

Data can be passed when calling the script:
```shell
./script.sh hello world
```

For example in the script:
```shell
echo $1
echo $2
```

then the result is:
```shell
$1 = hello
$2 = world
```

---
# Conditionals

Conditional helps the script make decisions:
```shell
if [condition]; then
  command
elif [condition]; then
  command
else [condition];
  command
fi
```

Example:
```shell
age=23

if [$age -ge 25]; then
  echo "Cece"
else
  echo "Sage"
```

The comparisons:
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

Loop is used to run a command multiple times.

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

| Command / Syntax | Meaning                |
|:-----------------|:-----------------------|
| `man command`    | view manual            |
| `info command`   | view documentation     |
| `gzip`           | compress `.gz`         |
| `bzip2`          | compress `.bz2`        |
| `xz`             | compress `.xz`         |
| `-d`             | decompress             |
| `zip`            | archive + compression  |
| `unzip`          | extract `.zip`         |
| `tar -cf`        | create archive         |
| `tar -xf`        | extract archive        |
| `tar -czf`       | tar + gzip             |
| `tar -cjf`       | tar + bzip2            |
| `tar -cJf`       | tar + xz               |
| `ping`           | check connectivity     |
| `host`           | DNS lookup             |
| `ifconfig`       | view network interface |
| `wget`           | download file          |
| `curl`           | transfer/download data |
| `#!/bin/bash`    | Bash shebang           |
| `var=value`      | create variable        |
| `$var`           | get variable value     |
| `read var`       | receive user input     |
| `$1`, `$2`       | script arguments       |
| `if ... fi`      | conditional            |
| `for ... done`   | loop                   |
| `while ... done` | conditional loop       |
| `alias`          | create alias           |