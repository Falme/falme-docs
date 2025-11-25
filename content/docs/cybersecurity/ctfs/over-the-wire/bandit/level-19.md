---
title: "Level 19"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 19

## Level Goal
> The password for the next level is stored in a file readme in the home directory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

## Commands you may need to solve this level
> ssh, ls, cat

## Steps

This challenge kick you out just after you connect with the bandit18 user, the .bashrc initializes a script that prints the text "Byebye!" and disconnects you. 

The way to solve this is to inject a command through ssh before it kicks you. Fortunately, the `ssh` command accepts a injected command inside the connection sequence. It's something like this:

```shell
ssh bandit.labs.overthewire.org -p 2220 -l bandit18 "cat ~/readme"
```

The `"cat ~/readme"` is a common command for the console, and it will run just after the connection was completed. In the case we want to read the content of the file `readme`.

Just with that, the password for the next level is revealed.
