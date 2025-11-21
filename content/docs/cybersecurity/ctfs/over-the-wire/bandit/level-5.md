---
title: "Level 5"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 5

## Level Goal
> The password for the next level is stored in the only human-readable file in the `inhere` directory. Tip: if your terminal is messed up, try the “reset” command.

## Commands you may need to solve this level
> ls , cd , cat , file , du , find

## Steps

(I know what happens in the next level, so we gonna play simple in this one)

The goal says that there's a human-readable file in the `inhere` directory with 
the password for the next level. This means that the file is not a file in 
machine code language. 

Machine code file is kinda garbage when you try to read it, so we will try to 
find a simple text file. First we enter the `inhere` folder with `cd`:

```shell
cd inhere
```

Listing the files in the folder with `ls` we have a list of files:

```
-file00
-file01
-file02
-file03
-file04
-file05
-file06
-file07
-file08
-file09
```

This time, we will use a command called `file`, this shows the details and 
information of that file. The problem here is that `file < -file00` does not 
work, this time we need to use another way to show the console we don't want 
to call an option, we will use `--`:

```shell
file -- -file00
```

So using the command `file` in each one of the files we have the following 
information:

```
-file00: data
-file01: OpenPGP Public Key
-file02: OpenPGP Public Key
-file03: data
-file04: data
-file05: data
-file06: data
-file07: ASCII text
-file08: data
-file09: data
```

The information we gathered follows:

- data : machine code, not human-readable
- OpenPGP Public Key : key for encryption, not human-readable
- ASCII text : character encoding standard, human-readable

With that, we have the file we want `-file07`, we read it with cat:

```shell
cat < -file07
```

Resulting with the password for the next level.
