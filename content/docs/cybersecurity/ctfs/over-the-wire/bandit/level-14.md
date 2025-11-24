---
title: "Level 14"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 14

## Level Goal
> The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Look at the commands that logged you into previous bandit levels, and find out how to use the key for this level.

## Commands you may need to solve this level
> ssh, scp, umask, chmod, cat, nc, install

## Steps

Normally we access a level using the `ssh` command with a password request, but this time we are using a private key. A secure connection using public and private keys are beyond the scope of this writeup, but I can assure that a private key should not be accessible for other users. This is the catch of this challenge, the private key is accessible and we are going to use it to access the next level.

At the home repository we can find the file `sshkey.private`. This is a RSA Private Key, and we can read it's content with the `cat` command, resulting in a bunch of encoded verification. We are going to copy this private key and save into our machine in a new file called `id_rsa`.

Now outside of the bandit user, we are going to connect to the next level with only the rsa key. The first place that the `ssh` verify for keys is at the path `~/.ssh`, so we are going to put our stolen private key there.

After that, just connect through `ssh` to the next level, so no password is necessary.

### WARNING: UNPROTECTED PRIVATE KEY FILE!

If this message appears to you, the problem is that the private key have too many permissions. It's a private file, so having too many permissions is a security problems, and the private key will be ignored. To solve that, change the permissions of the private key file using the `chmod` command:

```shell
chmod 600 id_rsa
```

Where:
- `chmod` : command to change permissions
- `600` : type of file permission, normally separated in User, Group and Others
	- `6` (User) : Read and Write
	- `0` (Group) : No Permission
	- `0` (Others) : No Permission
- `id_rsa` : private key file

Now, inside the next level, you have access to the password in /etc/bandit_pass/bandit14 .
