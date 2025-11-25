---
title: "Level 20"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 20

## Level Goal
> To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

## Commands you may need to solve this level
> 

## Steps

There's an executable file in the home folder named `bandit20-do`, to know more about that executable we will run it without arguments:

```shell 
./bandit20-do
```

It return to us the following text:

```
Run a command as another user.
	Example: ./bandit20-do whoami
```

The `whoami` command shows the name of the user currently logged in. If we use only the `whoami` command, it will return "bandit19", the user we logged in with `ssh`. But if the do `./bandit20-do whoami`, it will return "bandit20".

So, what we get from this is:

- bandit20-do executable can receive other commands
- bandit20-do acts like the bandit20 user

With that information, we will use the `bandit20-do` program to read the bandit20 password at /etc/bandit_pass/bandit20:

```shell
./bandit20-do cat /etc/bandit_pass/bandit20
```

With this, we have the password for the next level.
