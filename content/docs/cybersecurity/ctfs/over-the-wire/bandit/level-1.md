---
title: "Level 01"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 01

From now on, the challenge will begin, normally I use a spoiler section for the 
flag answer, but in the case of the OverTheWire, the password change 
occasionally, so I will only make a step-by-step guide with no final answer.

## Level Goal
> The password for the next level is stored in a file called readme located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

## Commands you may need to solve this level
> ls , cd , cat , file , du , find


## Steps

This level has the intention to teach you the basics of using a unix terminal 
or system. In my case I'm using a Linux in a Virtual Machine.

On the last level (level 0) we gained access to a user named bandit0. We can 
see this at the shell identification for the user currently logged in:

```shell
bandit0@bandit:~$
```

Reading the goal of this challenge, our objective is in a file named `readme`. 
So we first list all the files in the current folder with `ls`. It shows only a 
file named `readme`. That's the file containing the answer.

To read the content of the file we will use a command called `cat`, this will 
show it's content.

With that we have a text with the password for the next level.
