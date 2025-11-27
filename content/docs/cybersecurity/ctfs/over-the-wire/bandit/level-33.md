---
title: "Level 33"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 33

## Level Goal
> After all this git stuff, it’s time for another escape. Good luck!

## Commands you may need to solve this level
> sh, man

## Steps

This is the last challenge available at the moment, this one is another shell escapade and get the password inside the /etc/bandit_pass/ directory.

The shell that you are trapped in makes every command uppercase, which is a problem for linux, because the commands are case-sensitive, you cannot write any command without giving you an error. 

The solution that I found is to write `$0`, one of the special variables that runs the current shell, but in this case it redirects to another kind of shell, not locked in uppercase anymore. Inside this new shell, write `bash` to load the bash shell terminal.

Now, free from the uppercase terminal, we can go to `/etc/bandit_pass/bandit33` and read the next level password with `cat`.

