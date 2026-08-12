---
title: "Linux Command Line"
date: 2026-08-12
draft: false
tags: ["Linux"]
description: "Some basic Linux commands"
---

> In this section, I will mainly focus on the Bash shell.

---

# Linux Command Line

## 1. Shell Basics

### Shell

When entering the Linux terminal, there are two types of prompts that may appear. One starts with `$`, which means I am a normal user, while `#` means I am the root/superuser.

#### Syntax

```shell
command options arguments
```

#### Example

```shell
echo Hello World
```

When working with the terminal, some basic operations include:

- `Enter` to execute a command
- `Up Arrow` to go back to the previous command
- `Ctrl - C` to cancel a command if it gets stuck

---

## 2. Navigation

### pwd

Files and directories in Linux, as well as in most other operating systems, are organized in a tree hierarchy. You can imagine it as branches of a tree. This command helps me know where I am currently located.

#### Syntax

```shell
pwd
```

---

### cd

Simply used to move to another location. It can also be used with some special nodes for more specific navigation.

- With `.`, it stays in the current location
- With `..`, it moves back one level in the branch
- With `~`, it goes directly back to the home directory
- With `-`, it goes back to the last directory I was in

#### Syntax

```shell
cd .
cd ..
cd ~
cd -
```

---

### ls

Lists everything in the current location or in a specified directory.

You can add `-a` to view hidden files, `-l` to view more detailed information about each directory/file, or `-lh` to make it easier to read. `-r` is used to change the sorting order > these options can be combined, for example `ls -ltr` to sort by time and reverse the order.

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
-S > sort by file size
-d */ > display only the names of directories in the current location
```

---

## 3. File Management

### touch

The first purpose is to create a new file with the syntax:

```shell
touch [OPTIONS] FILE
```

The second purpose is to update the timestamp of a file.

Every file in Linux has 3 timestamps, including:

- atime is access time > the last time the file was read/opened
- mtime is Modification Time > the last time the file content was modified
- finally, ctime is Change Time > the last time the file information/metadata was modified.

#### Syntax

```shell
touch FILE > update the timestamp of the file
touch -r 1.txt 2.txt > copy the timestamp from one file to another
touch -d "2026-08-12 00:00" file > set a specific date and time for the file
touch -t 202608120000 file > set the time similarly to -d
touch -c FILE > update if the file exists and skip it, do not create it if it does not exist
touch -a > only update the access time
touch -m > only update the content modification time
```

---

### cp

Copy files from one location to another.

#### Syntax

```shell
cp _ _ _ /home/litaaya/
cp _ /home/litaaya/_
cp *.py /home/... > all files with the py extension
cp file? destination > all file1, file2, filex, but not file14
cp file[a-z] destination > fileb, filec, but not file3
cp -r dir/ /home/litaaya/doc > copy everything inside the dir directory and put it into doc
cp -i > prevent accidental overwriting, use this to be notified before overwriting
cp -f > force
cp -n > prevent overwriting
cp -p > copy and preserve the permission, ownership, and timestamp information of the original file
cp -a > take everything without leaving anything out
cp -u > only copy new files
cp -v > show the files being copied
```

---

### mv

#### Rename

```shell
mv old new
```

#### Move

Move files. Note that it will overwrite if a file already exists, so use `-i`/`n` or `-b` (backup), or `-v` to know what `mv` is doing.

```shell
mv file1 file2 /dir
mv -t dir/ file1 file2
```

---

### mkdir

Create a directory in the current location.

#### Syntax

```shell
mkdir _ _
mkdir -p _/_/_ > this is used to create deeper directory structures
mkdir -m > set up permissions for that directory
mkdir -v > print a message when creating that directory
```

---

### rm

It simply deletes _.

#### Syntax

```shell
rm _ _ _
rm -i _ > view information and confirm before deleting
rm -I _ > prompt for confirmation only once
rm -f > force
rm -r > delete a directory path
rmdir > also deletes a directory path, but it is safer because the directory must be empty before it can be deleted
rm -v > show what will be deleted
```

---

## 4. Viewing Files

### file

`file` helps me see what type of file something is, for example jpeg, txt, py, etc.

#### Syntax

```shell
file 1 2 3 4 > can check multiple files
file -i _ > view information in MIME and charset format according to a certain standard
file -b _ > remove the file name from the beginning of the output
file -L link > this will be used together with shortcuts that will be described later; briefly, it checks where the link points to and what type of file it is
file -z file.txt.gz > try to see what is inside this compressed file
```

---

### cat

When `cat` is used with `>`, it creates a new file. Note that if the file already exists, it will completely overwrite the existing file. If you want to append instead, use `>>`.

During use, you can press `Ctrl - -D` while on a new line to save and exit.

#### Syntax

```shell
cat file > print all information in the file to the screen, not highly recommended
cat file1 file2 > file3 -> this will print file1 and file2 if there is no > sign, and if there is, it will combine file1 and file2 into file3
cat -n file > add numbers to output lines
cat -b file > add numbers to non-empty output lines
cat -s file > squeeze multiple blank lines into a single blank line
cat -A file > show everything, for example ^I means there is a Tab in the file, while $ means Enter
```

---

### less

Open a new interface to view a file. Inside it, I can use the up and down arrow keys to view the file line by line, `g` to go to the beginning of the file, `G` to go to the end of the file, `u` to move up, `d` to move down, and `h` to view the available commands.

#### Syntax

```shell
/search_term > search for a word from top to bottom
?search_term > search for a word from bottom to top
n to move to the next result and N to go back to the previous result
q > exit
less -N file > show line numbers
less +G file > open at the last line of the file
less +F > show logs in real time, you can press ctrl C to stop the real-time update state
```

---

## 5. Command History

### history

View the history of commands I have executed.

For re-running commands, besides using the Up Arrow, I can use:

- `!!` to automatically run the most recent command again
- `!{number}` to run command number `{number}`
- `!cat` to run the most recent command that starts with `cat`

To search through the history, I can use `Ctrl R`, then `Enter` to use it.

#### Syntax

```shell
history -c > remove
history -w > save history
history -d <offset> > delete a command using its history number
```

---

## 6. Search

### find

Search for what I need.

#### Syntax

```shell
find dir -name _ > find _ inside dir and its subdirectories
find dir -iname _ > ignore case
find . -name "*.txt" > find txt documents
find dir -type d -name _ > go inside home and find a directory with the name _ > if searching for a file, use -type f
find . -type f -size +10M
find . -type f -size -1k
find . -type f -mtime -7 > within the last 7 days, while + means more than 7 days
```

`find` can be used together with `-print`, `-delete`, or `-exec`.

---

## 7. Getting Help

### help

When I forget a command, I use this.

#### Syntax

```shell
help _ > what this is, used for built-ins
_ --help > used for external binaries
man _ > open the detailed manual page of the command
whatis _ > print a one-line description of what the command does
type _ > find out whether this command should use help or --help
```

---

### man

Open the manual page. You can use:

- `/` to search
- `q` to quit
- `n` and `N` to move between information

Man page sections include:

- `1` for user commands
- `2` for system calls
- `3` for library functions
- `5` for file formats
- `8` for system administration commands

---

## 8. Shell Customization

### alias

Create a temporary alias.

#### Syntax

```shell
alias ll='ls -la' > using ll is equivalent to using ls -la
unalias ll > remove ll
alias > list the current aliases
type ll > find out what ll does
```

To make it permanent, access:

```shell
nano ~/.bashrc
```

And write this inside:

```shell
alias ll='ls -la'
```

---

## 9. Exit

### exit

As the name suggests, it helps me exit.

#### Syntax

```shell
exit
logout
Ctrl - D
```