---
title: "Level 03"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 03

## Level Goal
> The password for the next level is stored in a file called `--spaces in this filename--` located in the home directory

## Commands you may need to solve this level
> ls , cd , cat , file , du , find

## Steps

This one is similar to the previous level (level 2), but in this case the 
special character is the space character. Normally the space after calling a 
program is a separator of arguments. So, let's use an example with `cat`:

```shell
cat file.txt
```

So in this case we have the program `cat` and an argument expected separated by 
a space, a file named `file.txt`.

Reading a file with space in the name cause confusion to the console, it can't 
separate what is an space and what is an argument. To make it work we, 
surprisingly, have two ways to read the file.

### Quotes

We can use quotes or double quotes around the path, so space is inside quotes 
and arguments is inside the quotes. It will be something like:

```shell
cat < "--spaces in this filename--"
```

(The `<` is necessary, since `-` is also a special character)

### Backslash

Backslash is also an special character, but it's used to "Escape a Character", 
making a space be read as a space and not a separator. It will be something 
like:

```shell
cat < --spaces\ in\ this\ filename--
```

So every space in the path should be replaced with backslash+space. 
With this, the answer to the next level is revealed.
