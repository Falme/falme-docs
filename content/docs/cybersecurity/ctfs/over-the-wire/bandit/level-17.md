---
title: "Level 17"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 17

## Level Goal
> The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL/TLS and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.
> 
> Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.

## Commands you may need to solve this level
> ssh, telnet, nc, ncat, socat, openssl, s_client, nmap, netstat, ss

## Steps

In this challenge, the way to solve it is the same as the last level (level 16), but the port that we need to connect is unknown in a range of 1000 ports (from 31000 to 32000), we need to find the correct port and connect to them via `ncat --ssl`.

To find a port in a range, we will use a tool called `nmap`, this tool is a scanner of many things, and also a port scanner. We will use `nmap` to scan those 1000 ports and check which one is open and which one is closed. The command will be something like:

```shell
nmap localhost -p 31000-32000
```

Where:
- `nmap` : is the scanning tool
- `localhost` : is the address for the server
- `-p` : is the option to look through specific ports
- `31000-32000` : is the range of ports to scan, the dash means "between"

With that, we have the result of 5 ports open, but with no information. We know that the correct port only accepts SSL connection type, so we will use the same as before with `ncat`:

```shell
ncat --ssl localhost {port}
```

Normally, the wrong port will result in an error, and the correct one will return nothing. Just put the current level password and it will return with the next level RSA credentials.

Save the credentials to a new file, and use the command `ssh` to connect without need of a password:

```shell
ssh -i ./key-saved.private bandit.labs.overthewire.org -p 2220 -l bandit17
```

Any questions, checkout the level 14 challenge.
