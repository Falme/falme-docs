---
title: "Level 29"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 29

## Level Goal
> There is a git repository at ssh://bandit28-git@bandit.labs.overthewire.org/home/bandit28-git/repo via the port 2220. The password for the user bandit28-git is the same as for the user bandit28.
> 
> Clone the repository and find the password for the next level.

## Commands you may need to solve this level
> git

## Steps

Now, do the same as the last level (level 28) to clone the repository into a folder, but now for the bandit28-git.

After that, we can check that the file `README` does not contain the password for the next level. Remember that I said `git` have a timeline of changes in the project? Well, this is the time to check what was changed. Let's do the command:

```bash
git show
```

This command will show what was the changes in the file `README`, and also revealing the password for the next level.

