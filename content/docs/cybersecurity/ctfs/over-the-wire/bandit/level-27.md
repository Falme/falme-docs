---
title: "Level 27"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 27

## Level Goal
> Good job getting a shell! Now hurry and grab the password for bandit27!

## Commands you may need to solve this level
> ls

## Steps

We still have unsolved problems for bandit26, so we need to make the same steps as the last level (level 26) until the visual mode for vim is enabled (after pressing `v` for editing).

Just then, we need to run a program named `bandit-27-do`, but we cannot access with the current terminal. So, we are going to open a new terminal within the vim editor with the command:

```
:term bash
```

With this command we are going to open a separated bash shell. Now we can execute `bandit27-do`, which is similar to `bandit20-do`, it pretends that we are bandit27 and run a command. So, we open the bandit27 password with:

```bash
./bandit27-do cat /etc/bandit_pass/bandit27
```

Now, the password for the next level is revealed.
