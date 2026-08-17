---
title: "Linux Text Processing"
date: 2026-08-17
draft: false
tags: ["Linux"]
description: "Basic knowledge of text processing"
---
---
# stdout - standard output

`stdout` is the normal output of a process, displayed on the terminal by default.
```shell
echo "hello"
```

Redirect output to a file
```shell
echo "hello" > file.txt
echo "hello" >> file.txt
```

`>`: write output to a file, overwrite the old content.

`>>`: append to the end of the file.

---
# stdin - standard input

`stdin` is the default input of a process.

```shell
cat < file.txt
```

Flow: `keyboard/file` -> `stdin` -> `command`.

---
# stderr - standard error

`stderr` is the stream used for error messages, separate from `stdout`.

Linux has basic file descriptors such as:
```shell
0 = stdin
1 = stdout
2 = stderr
```

Example:
```shell
command 2>> error.txt
command 1> output.txt 2>&1
```

---
# pipe and tee

`pipe` sends the `stdout` of the previous command to the `stdin` of the next command.
```shell
command1 | command2
ls -l | grep ".txt"
```

`tee` outputs to the terminal while also writing the output to a file.
```shell
ls | tee file.txt
ls | tee -a file.txt
```

---
# env - environment

Environment variables contain values that the shell or process can use.
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

`cut` is used to extract part of each line of text.

By character, for example, take the first character of each line:
```shell
cut -c 1 file.txt
```

Character range:
```shell
cut -c 1-5 file.txt
```

By field:
```shell
cut -d ',' -f 2 data.csv
```

`-d`: delimeter.

`-f`: field.

`-c`: character.

---
# paste

`paste` combines corresponding lines from multiple files. By default, the seperator is `Tab`. In the example below, `-d` uses a delimeter while `-s` combines the lines of a file into one line.
```shell
paste -d ',' file1.txt file2.txt
paste -s file.txt
```

---
# head and tail

`head`/`tail` are used to view the beginning/end of a file, respectively. The default is 10 lines.
```shell
head file.txt
head -n 5 file.txt
head -5 file.txt

tail file.txt
tail -n 5 file.txt
tail -5 file.txt
```

With `tail`, you can use:
```shell
tail -f app.log
```

`-f` can be used to follow the file and new lines as they are written.

---
# expand and unexpand

`expand` converts `Tab` to `Spaces`, while `unexpand` does the opposite.

You can adjust the `Tab` size:
```shell
expand -t 4 file.txt
```

---
# join and split

```shell
join file1.txt file2.txt
```

It is similar to `JOIN` in sql, but much simpler and requires the data to already be properly organized.
```shell
split file.txt
split -l 1000 bigfile.txt
```

Split into 1000 lines per file

---
# sort

`sort` is used to sort lines of text.
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

`tr` is used to change or remove characters from the input stream
```shell
echo "hello" | tr 'a-z' 'A-Z'
```
will produce the result `HELLO`.

```shell
echo "a,b,c" | tr ',' ' '
```
will produce the result `a b c`.

```shell
echo "hello123" | tr -d '0-9'
```
will produce the result `hello`.

---
# uniq - unique

`uniq` removes adjacent duplicate lines

> `uniq` only handles adjacent duplicates, so it usually needs to be combined with `sort`.
```shell
sort file.txt | uniq
sort file.txt | uniq -c
sort file.txt | uniq -d
```

`-c`: count.
`-d`: duplicates > show each dup.

---
# wc and nl

`wc`: word count, used to count content. It can be used with `-l` for the number of lines, `-w` for the number of words, and `-c` for the number of bytes.
```shell
wc -l file.txt
wc -w file.txt
wc -c file.txt
```

`nl` adds line numbers to a text file.
```shell
nl file.txt
```

---
# grep

`grep` is used to find a line that matches a pattern.
```shell
grep "error" app.log
grep -i "error" app.log
grep -n "error" app.log
grep -v "DEBUG" app.log
grep -r "password"

ps aux | grep python
```

`-i`: ignore case.
`-n`: line number.
`-v`: invert.
`-r`: recursive.

---
# Cheatsheet

| Command/symbol | Meaning                                       |
|:---------------|:----------------------------------------------|
| `stdout`       | standard output                               |
| `stdin`        | standard input                                |
| `stderr`       | standard error                                |
| `>`            | stdout -> file, overwrite                     |
| `>>`           | stdout -> file, append                        |
| `<`            | file -> stdin                                 |
| `2>`           | stderr -> file                                |
| `2>>`          | append stderr                                 |
| `\|`           | stdout previous command -> stdin next command |
| `tee`          | output to terminal and file                   |
| `env`          | view environment variables                    |
| `$VAR`         | get the value of an environment variable      |
| `export`       | put a variable into the environment           |
| `cut`          | get characters/fields                         |
| `paste`        | combine corresponding lines                   |
| `head`         | view the beginning of a file                  |
| `tail`         | view the end of a file                        |
| `tail -f`      | follow a file in realtime                     |
| `expand`       | tabs -> spaces                                |
| `unexpand`     | spaces -> tabs                                |
| `join`         | join two files by field                       |
| `split`        | split a file into multiple parts              |
| `sort`         | sort lines                                    |
| `tr`           | change/delete characters                      |
| `uniq`         | remove adjacent duplicate lines               |
| `wc`           | count lines/words/bytes                       |
| `nl`           | number lines                                  |
| `grep`         | find/filter by pattern                        |


```shell
0 = stdin
1 = stdout
2 = stderr
```