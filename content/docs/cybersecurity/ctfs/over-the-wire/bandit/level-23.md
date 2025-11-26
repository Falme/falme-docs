---
title: "Level 23"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 23

## Level Goal
> A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.
> 
> NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

## Commands you may need to solve this level
> cron, crontab, crontab(5) (use “man 5 crontab” to access this)

## Steps

The steps to reach the script that is running in the `cron` routine is the same as the last level (level 22) :

- Open the /etc/cron.d/ folder with `cd`
- Read the file content of `cronjob_bandit23`
- Go to path of the script inside it (`cronjob_bandit23.sh`)
- Read the script

This is a pretty simple script, it takes the current user logged in with `whoami` command, encode the result with a `md5sum` encoder and trim the unnecessary characters, the result of this encoding is the name of the file in the /tmp/ folder that contains the password for the next level.

How to find the result is just a case of repeating the script steps correctly until you reach the correct file path, it's something like:

```shell 
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
```

Where
- `echo I am user bandit23` : print a string to be redirected to `md5sum` and bandit23 is the pretended user
- `md5sum` : encode in MD5 the last string
- `cut -d ' ' -f 1` : removes the extra unnecessary characters

Reading the file in the found path, the next level password is revealed.
