---
title: "Level 32"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 32

## Level Goal
> There is a git repository at ssh://bandit31-git@bandit.labs.overthewire.org/home/bandit31-git/repo via the port 2220. The password for the user bandit31-git is the same as for the user bandit31.
> 
> Clone the repository and find the password for the next level.

## Commands you may need to solve this level
> git

## Steps

First of all, clone the repository just like in the previous levels. After that we can continue the journey to find the password for the next level.

In this challenge we are going to learn about the `.gitignore`, this is a file that dictates what's NOT being tracked in the project. Maybe a binary, a file you don't want to send to the repository, etc.

After cloning the repository, you can find the `.gitignore` and read it with `cat`, it shows that every "*.txt" file is being ignored, so no .txt file will be tracked.

Then, there's the README.md file, the text in it says:

```
This time your task is to push a file to the remote repository.

Details:
	File name: key.txt
	Content: 'May I come in?'
	Branch: master
```

So, we need to create a file named `key.txt` with the text inside as "May I come in?" on the master branch. But, the `.gitignore` does not track the .txt files, so we cannot push this file.

So, we are going to delete the .gitignore and commit our changes, without the .gitignore, there's nothing to deny us to send the .txt file.

So send a change in `git`, usually we go through this pipeline:

- `git add ./file` : We add the file to the stage, preparing the file to the commit
- `git commit -m "message"` : We commit our changes and files, usually we also write a message to know what changed
- `git push --all` : Then we push our changes and files to the server where the repository is located.

With the new file, it will be something like this:

```shell
git add ./key.txt
git commit -m "May I come in?"
git push --all
```

If you did everything right, `git` will throw an error with the password for the next level. 
