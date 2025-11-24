---
title: "Level 00"
# weight: 0
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 00

Hello and welcome to my journey through Over The Wire, one of my favorites CTFs.

I've already completed this challenge years ago, but I decided to return to it 
to revisit it, and this time writing about the solutions.

Mind that this documentation is for me only, if you didn't solve this ctf by 
yourself, do not read this posts, just go and solve these challenges for 
yourself. Do not spoil your experience.

## Level 00

> The goal of this level is for you to log into the game using SSH. The host to which you need to connect is bandit.labs.overthewire.org, on port 2220. The username is bandit0 and the password is bandit0. Once logged in, go to the Level 1 page to find out how to beat Level 1.

This level is an introduction to the system that we will dive in: Bandit.

So the solution is present in the problem, it says which are the login and 
password to login into the game. The only problem is how to use the SSH 
command.

In this case, it will be something like:

```shell
ssh bandit.labs.overthewire.org -p 2220 -l bandit0
```

Where:
- `ssh` : Is the SSH command call 
- `bandit.labs.overthewire.org` : Is the host to connect
- `-p 2220` : Is the door to connect, in the case of bandit, is always 2220
- `-l bandit0` : Is the username that is connecting, in this case, bandit0

So, with that, we finished level0. Now we can go to Level1.
