---
title: "Level 8"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 8

## Level Goal
> The password for the next level is stored in the file data.txt next to the word millionth

## Commands you may need to solve this level
> man, grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

## Steps

This challenge have the intention to teach us the `grep` command. `grep` print lines with a match pattern, in this example, a string.

Imagine you are looking through a big file and you need a specific word, grep will ignore other lines and only shows you the one you need.

Why this is necessary, you say? First of all, it's practical. Second, you can automate some searches from command line, but third and most important to me, to find patterns in huge files.

Some files, like lists and dictionaries, is just a text file (.txt) with gigabytes in size. If you try to open it with notepad it's not gonna work. So, to play with it, let's go back to the Level Goal.

We want to find the word "millionth", so using `grep`, it's something like:

```shell
cat data.txt | grep 'millionth'
```

Where:
- `cat` : opens the file
- `data.txt` : the file to read the data
- `|` : pipe special character, where it send one command output to another command input
- `grep` : command to filter the output of text from `cat data.txt`
- "millionth" : the string we want to filter

With this, the password for the next level will appear next to the word millionth.
