---
title: "Level 28"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 28

## Level Goal
> There is a git repository at ssh://bandit27-git@bandit.labs.overthewire.org/home/bandit27-git/repo via the port 2220. The password for the user bandit27-git is the same as for the user bandit27.
> 
> Clone the repository and find the password for the next level.

## Commands you may need to solve this level
> git

## Steps

From now on, we are going to play with `git`. Git is a version control program, it uses repositories to track changes in a project, creating a timeline of changes in a effective way and with great performance.

This is the basic of dealing with `git`, we are going to clone a repository into a folder, which is making a copy from a server into our machine. The command will be something like:

```bash
git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
```

Remember that we are dealing with a different port (2220), the common ssh port is 22, so we needed to add it to the ssh address.

After cloning the repository, find the file README, it contains the password for the next level.
