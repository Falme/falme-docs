---
title: "Level 31"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 31

## Level Goal
> There is a git repository at ssh://bandit30-git@bandit.labs.overthewire.org/home/bandit30-git/repo via the port 2220. The password for the user bandit30-git is the same as for the user bandit30.
> 
> Clone the repository and find the password for the next level.

## Commands you may need to solve this level
> git

## Steps

First of all, clone the repository just like in the previous levels. After that we can continue the journey to find the password for the next level.

This time the `git log` and `git show` have only one file, with no changes. This seems to be a wall, maybe there's a different branch, but no branch bring us to the password. 

Actually the answer is on the filesystem inside the hidden folder `.git`, it shows some packed files, so, let's check the refs for the packages in the repository. Open the `.git/packed-refs` files with `cat` to show a weird tag place:

```
843...351 /refs/tags/secret
```

The first hash is probably a commit hash on a tag called secret. So we are going to use a command called `git cherry-pick` to get the specific commit from the tree.

```
git cherry-pick 843...351
```

Then `git` return to us a message: "Can't Cherry-pick a blob". Ok, this means that we are not dealing with a commit, but with a blob of some kind. Reaching the `git` manual, we can find a command that deals with that for us called `unpack-file`, saying:

```
git-unpack-file(1)
           Creates a temporary file with a blob’s contents.
```

So we are going to do the following command:

```shell 
git unpack-file 843...351
```

After this, a file will be created in the same folder named `.merge_file_uEpLe8`, then use `cat` to read the unpacked file, revealing the password for the next level.
