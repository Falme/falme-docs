---
title: "Level 02"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 02

## Level Goal
> The password for the next level is stored in a file called - located in the home directory

## Commands you may need to solve this level
> ls , cd , cat , file , du , find

## Steps

In this case the file is literally called `-`, you can list the file to be 
shown using the command `ls` and you can see the file is there.

But it's not possible to just use the command `cat -` to read the file. See, 
the dash character is a special character, it's used to define options into a 
program. Let's see another example:

```shell
ls -l 
```

In this case, the command `ls` is using an option `-l`, which list files in a 
folder in a long list format.

So we cannot use `cat -`, because cat is selecting an option to be executed. To 
make this work we will need an alternative (and the reason of this challenge), 
because cat is waiting an input of a file, we will use some I/O redirection.

First let's explain the Input/Output redirection. If we make something like 
`echo "test text" > file.txt`, this means that we will write into a console 
using `echo` the text `"test text"`, and then redirect the Output into a file 
named `file.txt` using the `>` signal.

Ok, if the `>` means output, the `<` means input, right?

So let's Input the file `-` into the `cat` command, it will be something like:

```shell
cat < -
```

And with that, we have the password for the next level.
