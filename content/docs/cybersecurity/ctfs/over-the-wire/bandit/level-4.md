---
title: "Level 04"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 04

## Level Goal
> The password for the next level is stored in a hidden file in the `inhere` directory.

## Commands you may need to solve this level
> ls , cd , cat , file , du , find

## Steps

This challenge teaches us about hidden files and directories. In Linux, hidden 
files is marked with a dot (`.`) before the file name. This makes it marked as 
hidden and will not show in a common `ls` command.

So if we use ls in the home directory, we can find the directory `inhere`. Then 
we enter the `ìnhere` directory using the `cd` command:

```shell
cd inhere
```

Now, inside `inhere` directory, if the list the files with `ls`, no file can be 
found. To show hidden files inside a directory we will use the option `-a` to 
show all files, including hidden files. It will be something like:

```shell
ls -a
```

With that, the file `...Hiding-From-You` is revealed, reading the file with `cat`:

```shell
cat < ...Hiding-From-You
```

Reveals the password for the next level.
