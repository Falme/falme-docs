---
title: "Level 16"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 16

## Level Goal
> The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL/TLS encryption.
> 
> Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.

## Commands you may need to solve this level
> ssh, telnet, nc, ncat, socat, openssl, s_client, nmap, netstat, ss

## Steps

This challenge is very similar to the previous one (level 15), but this time we need to make a secure connection using SSL/TLS encryption. For that, we need a more powerful Netcat, so we are going to use the `ncat` command with the option `--ssl`:

```
--ssl                  Connect or listen with SSL
```

Making the secure connection to the localhost at port 30001, like this:

```shell
ncat --ssl localhost 30001
```

And after making the connection, we put the current level password and press enter. With that, the password for the next level is revealed.
