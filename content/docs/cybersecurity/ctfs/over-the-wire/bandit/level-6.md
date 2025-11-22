---
title: "Level 6"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 6

## Level Goal
> The password for the next level is stored in a file somewhere under the `inhere` directory and has all of the following properties:
>
>    human-readable  
>    1033 bytes in size  
>    not executable  

## Commands you may need to solve this level
> ls , cd , cat , file , du , find

## Steps

This time we are going to use the power of Linux and the `find` command. For this challenge, I recommend you to read the manual for `find` command, using `man find`, this will be very useful.

We are looking for a file that is human-readable, 1033 bytes and non-executable. For this, we will use `find` with some filtering parameters. But first, to play with it, we will just list all files, no need to open and enter the `inhere` folder:

```shell
find ./
```

This command will list all files in the current folder (that `./` means current folder) in a recursively way. What we need now is to filter the results to be the exact parameters we are looking for. Checking the manual for `find` we can find the `-executable` option:

```
-executable
              Matches files which are executable and directories which
              are searchable (in a file name resolution sense) by the
              current user.
```

Ok, now we can filter the results to be only the executable files, but that's not what we are looking for, we are looking for files that are NOT executable. Returning to the manual, we can find the `-not` option, that inverts the result of an option:

```
-not expr
              Same as ! expr, but not POSIX compliant.
```

Making use of these options, we can list all the non-executable files inside `inhere`:

```shell
find ./ -not -executable
```

But still, there's too many files to be searching by hand, we need to filter better. This time we will filter by size, looking for files that have exactly 1033 bytes. Checking the manual for `find` again, we can find the `-size` option:

```
-size n[cwbkMG]
              File uses less than, more than or exactly n units of space,
              rounding up.  The following suffixes can be used:

              `c'    for bytes
```

With the size option, we can add it to our command and check for the results:

```shell
find ./ -not -executable -size 1033c
```

With this command, the result is filtered to only one file:

```
./inhere/maybehere07/.file2
```

Reading the file with the `cat` command, the password for the next level is revealed.
