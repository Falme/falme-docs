---
title: "Level 21"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 21

## Level Goal
> There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).
> 
> NOTE: Try connecting to your own network daemon to see if it works as you think

## Commands you may need to solve this level
> ssh, nc, cat, bash, screen, tmux, Unix ‘job control’ (bg, fg, jobs, &, CTRL-Z, …)

## Steps

This challenge teaches us about connections between two places that the netcat can do (Not that much in this case, because both will be running in the localhost, but whatever...). 

So, we have a new executable in our home directory named `suconnect`, this one makes a connection to a port and waits for an information to come. To make this a little easier to understand, we will open two consoles, both connected in the user "bandit20" (If you prefer to use `tmux` or similar, no problem). We are going to call them "ConsoleA" and "ConsoleB".

At the "ConsoleA" we will start a Listening connection of netcat at a random port, let's just say we use the port `4321`, it will look something like this:

```shell 
nc -l localhost 4321
```

Where:
- `nc` : netcat program for connection
- `-l` : listening option, so we can listen for the new password
- `localhost` : server to listen
- `4321` : random port

Now the "ConsoleA" is running netcat on listening mode, now, we go to "ConsoleB".

On "ConsoleB" we are going to execute the `suconnect` program at the same port as the netcat, it will be something like this:

```shell
./suconnect 4321
```

Now, both of the Consoles will be active, we just need to process this. This is going to work the following way:

1. We input the password of bandit20 into "ConsoleA", running netcat
2. The "ConsoleB", running `suconnect`, will receive the string from netcat
3. The "ConsoleB", running `suconnect` will verify if the password is correct
4. If the password is correct, it will send the new password back to "ConsoleA", running netcat

With that, the password for the next level is revealed.
