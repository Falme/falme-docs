---
title: "Level 00"
# weight: 0
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Leviathan - Level 00

Hello and welcome to my journey through Over The Wire, one of my favorites CTFs. Now tackling the Leviathan section, dedicated to the first steps of reverse engineering.

Mind that this documentation is for me only, if you didn't solve this ctf by yourself, do not read this posts, just go and solve these challenges for yourself. Do not spoil your experience.

## Steps

This is the first challenge, it's very simple, and after doing the Bandit section, this could appear as obvious. First, we connect to the ssh of overthewire with leviathan0 user. Here's the credentials for the first challenge:

```
Username: leviathan0
Password: leviathan0
```

To make an access through ssh, it'll look something like this:

```shell
ssh leviathan.labs.overthewire.org -p 2223 -l leviathan0
```

When asking the password type the password `leviathan0`. Now you are connected, putting us directly into the home directory `~`.

Using the command `ls -la` we can find a hidden folder named ".backup", probably from a system backup. Inside it we have a html file named "bookmarks.html", probably a bookmarks backup from an older browser. So, let's check the content with `cat`.

```shell
cd .backup
cat bookmarks.html
```

With that a bunch of text is displayed, that's normal for a backup file, and at the header of the file we can find a reference to netscape, an older browser.

Even with that many lines of text, we must find what we are looking for, the password for the next level. So, we are going to use `grep` to filter some words, maybe we can find what we are looking for:

```shell
cat bookmarks.html | grep "pass"
```

Searching for the word "pass" (as in "password"), we have a result from the file, a "Fix later" that have the password for the next level.
