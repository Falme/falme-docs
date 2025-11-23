---
title: "Level 7"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 7

## Level Goal
> The password for the next level is stored somewhere on the server and has all of the following properties:
>
>    owned by user bandit7  
>    owned by group bandit6  
>    33 bytes in size  

## Commands you may need to solve this level
> ls , cd , cat , file , du , find , grep

## Steps

This challenge is a next step from the last level (level 6), the solution is practically the same as before, but some different parameters.

First of all, the goal is asking to search a file somewhere in the server, that means we are not limited to the `inhere` or home directory. So, we are looking through the files in `/`, that is the root directory, from that, we can recursively find all possible files in the system.

So, let's start with the options in the `find` command we already know, that is the `-size` option:

```shell
find / -size 33c
```

This command will look through all the system trying to find a file with 33 bytes in size. The results show too much files, so we need to filter better to find the correct file. Looking through the manual, we can find the `-user` option:

```
-user uname
              File is owned by user uname (numeric user ID allowed).
```

This command permit us to filter the files owned by bandit7 user.

```shell
find / -size 33c -user bandit7
```

Still showing too many possible files, so we need to filter by group as well. Looking through the manual of `find`, we can find the `-group` option:

```
-group gname
              File belongs to group gname (numeric group ID allowed).
```

This command permit us to filter the files in the bandit6 group.

```shell
find / -size 33c -user bandit7 -group bandit6
```

With this you can already find the file with the password for the next level. But, there's too many files showing in the results that we don't have permission to access. So we are doing some cleaning here, so only the correct file will show to us.

### File Descriptors

We already learned about redirection with `<` and `>` special characters, know we will learn about File Descriptors. The different type of File Descriptors are:

- stdin: Represented by the number 0, is the Input
- stdout: Represented by the number 1, is the common print output
- stderr: Represented by the number 2, is the error print output

In our case, we want to remove the Permission errors in the output of our `find` command. So we are using the stderr (2) in the example.

Going through the command before, we are adding the following redirection:

```shell
2>/dev/null
```

Explaining a bit the structure:

- `2` : is the stderr, or error, that the command will output, like a tag from the output print
- `>` : is the redirection, we want the error messages to go elsewhere, anywhere but our console
- `/dev/null` : is the path to a special file, used for discarding informations, we want to discard the errors in this case

And with that we discarded the error messages. In the end the final command will be something like:

```shell
find / -size 33c -user bandit7 -group bandit6 2>/dev/null
```

With the result the correct file is revealed, using `cat` to read the file shows the password for the next level.
