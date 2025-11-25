---
title: "Level 18"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 18

## Level Goal
> There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new
> 
> NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19

## Commands you may need to solve this level
> cat, grep, ls, diff

## Steps

This challenge is meant to teach us about the `diff` command. The `diff` compares two files and output us the difference between them, very commonly used with `git` and others version control programs. The idea is that there's an old file and a new file, it will show you what changed between those 2.

The character "<" is a difference that exited the file, and the character ">" is a difference that entered the file. So, let's compare the files:

```shell
diff passwords.old passwords.new
```

The password for the next level should be next to the ">" character.
