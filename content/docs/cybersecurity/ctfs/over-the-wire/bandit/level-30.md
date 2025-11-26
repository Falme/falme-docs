---
title: "Level 30"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 30

## Level Goal
> There is a git repository at ssh://bandit29-git@bandit.labs.overthewire.org/home/bandit29-git/repo via the port 2220. The password for the user bandit29-git is the same as for the user bandit29.
> 
> Clone the repository and find the password for the next level.

## Commands you may need to solve this level
> git

## Steps

Clone the repository just like in the previous level, and we are going to find the password for this challenge.

This time the challenge teach us about branches. In `git` branches are different timelines of a project, maybe you want to test a feature, so you make a new branch to test this feature in a non-destructive way. So, first we are going to check how many branches are available. We can check with:

```shell 
git branch --all
```

This will show all the branches available in the project. We are going to check the development branch called `origin/dev`, to change the branch to that one, we will use the `git checkout`:

```shell 
git checkout origin/dev 
```

Now, we changed from the branch master to origin/dev, opening the README.md file with `cat` now contains the password for the next level.
