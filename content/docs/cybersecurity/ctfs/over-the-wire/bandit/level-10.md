---
title: "Level 10"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 10

## Level Goal
> The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several '=' characters.

## Commands you may need to solve this level
> grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

## Steps

Most of the file `data.txt` is filled with machine code, this means that is not human-readable, we could use the command `xxd` to read the file and find manually for the password, but there's too many content, and will take us too much time.

So, we are going to think this way, we only need the readable part of the file, in another words, only the strings in the file. So we are going to use the `strings` command, where it shows only the strings inside a file and nothing more. Making something like:

```shell
strings data.txt
```

Will show only the strings in the file. But it's still too many content. In the Goal text it says that the password is preceded by several '=' characters, so we can use the `grep` command to filter the several '=' characters. But how many '=' is several? Let's try 2, looks enough for the search. It looks something like:

```shell
strings data.txt | grep '=='
```

And with that, the password for the next level is revealed.
