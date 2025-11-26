---
title: "Level 26"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 26

## Level Goal
> Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.
> 
>    NOTE: if you’re a Windows user and typically use Powershell to ssh into bandit: Powershell is known to cause issues with the intended solution to this level. You should use command prompt instead.

## Commands you may need to solve this level
> ssh, cat, more, vi, ls, id, pwd

## Steps

This one gave me problems the first time I solved it, you need to think outside of the box, or about the size of the box.

The problem is, in the home folder, there's a rsa private key to login into bandit26 user, but after the login it disconnects. We need to check other things first.

The goal text says that bandit26 is not using /bin/bash for the shell, so we need to check which shell it's using. For that we need to check the information for all the users in the system. Let's check the `passwd` file.

```shell 
cat /etc/passwd | grep bandit26
```

This file contains the path of a shell script named `showtext`, showing it contents with `cat` reveals the simplicity of the script. It just set an environment, print the presentation text on screen and exit the terminal.

The trick is on the line reading the presentation text. It uses the `more` command, and we will use that to our favor.

First, make the terminal height very small, like 2 or 3 lines in height, then connect normally to the bandit26 user. Half of the presentation text should appear, because the `more` command will pause the process of the script, waiting for the user input.

In this moment, press the `v` key, it will go to editing/visual mode for vim. Now you are in an text editor. We will now open the password for the bandit26 level. Type the following still in vim:

```shell 
:e /etc/bandit_pass/bandit26
```

This will open the file directly in the terminal. Copy the password for the next level and close the vim with `:qa` (quit all)
