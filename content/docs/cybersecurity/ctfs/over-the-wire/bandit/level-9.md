---
title: "Level 9"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 9

## Level Goal
> The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

## Commands you may need to solve this level
> grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

## Steps

We need to find an unique line in the data.txt file that does not repeat, because this file is filled with possibilities for a password. The first instinct is to use the `uniq` command, because, in it's description:

```
uniq - report or omit repeated lines
```

And it's correct to use the `uniq` command, that only shows lines that are unique and not repeated. But, in the end of the `uniq` manual page, we have:

```
'uniq' does not detect repeated lines unless they are adjacent.
       You may want to sort the input first, or use 'sort -u' without
       'uniq'.
```

So, before we use the `uniq` command to filter the repeated lines, we need to sort the lines alphabetically, and then use the `uniq` command. So the command will be something like:

```shell
sort data.txt | uniq -u
```

Where:
- `sort` : sort the lines alphabetically
- `data.txt` : file to be sorted and processed
- `|` : pipe special character, to send the sorted content to the `uniq` command
- `uniq` : command to report or omit repeated lines
- `-u` : option of `uniq` to only print unique lines

With that, only the password to the next level will be printed.
