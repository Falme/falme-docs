---
title: "Level 11"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 11

## Level Goal
> The password for the next level is stored in the file data.txt, which contains base64 encoded data

## Commands you may need to solve this level
> grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

## Steps

This exercise is the first one that have (and teaches us) about cryptography. Not that we need to understand cryptography, but we need to decrypt a file. The file `data.txt` is encrypted in base64, which have a command `base64`. In the manual for `base64` we have the option `-d`:

```
-d, --decode
              decode data
```

So, we use this option to decode the file `data.txt`, it will be something like:

```shell
base64 -d data.txt
```

And with that, the password for the next level is revealed.
