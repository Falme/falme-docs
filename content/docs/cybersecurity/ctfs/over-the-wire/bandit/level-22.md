---
title: "Level 22"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 22

## Level Goal
> A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

## Commands you may need to solve this level
> cron, crontab, crontab(5) (use “man 5 crontab” to access this)

## Steps

This time, we are going to the `cron` config files. `cron` is a scheduler, it runs in the background to do something from time to time. The tasks can be found in the path /etc/cron.d/, so we are going to take a look there.

We can find some files, but at the moment the file `cronjob_bandit22` is the best to our goal. Using the `cat` command to read the file, we can find the next path:

```
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

We know that /dev/null is a special discard file, but something may be in the /usr/bin/cronjob_bandit22.sh script that help us. So, we use `cat` again to check it out what it does.

It writes from time to time the password from /etc/bandit_pass/bandit22 into a file in the /tmp/ folder. Opening this file in the /tmp/ folder, the password for the next level is revealed.

