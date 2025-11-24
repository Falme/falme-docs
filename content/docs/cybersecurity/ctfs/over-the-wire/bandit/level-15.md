---
title: "Level 15"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 15

## Level Goal
> The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

## Commands you may need to solve this level
> ssh, telnet, nc, openssl, s_client, nmap

## Steps

Understanding the goal text, the port 30000 is being listened, waiting for an input of the current level password. To make a connection TCP/UDP through a listening port, we will use the command `nc`. The command `nc` (or netcat) is made for this kind of work, so, we are going to make a connection with:

```shell
nc localhost 30000
```

No answer will be displayed in the console, but it's waiting for the input. Just write the password of the current level, press enter and the password for the next level will be shown.
