---
title: "Level 13"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 13

## Level Goal
> The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the command “mktemp -d”. Then copy the datafile using cp, and rename it using mv (read the manpages!)

## Commands you may need to solve this level
> grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd, mkdir, cp, mv, file

## Steps

This challenge is kinda weird for me, I don't know if I missed something to make it easier, but it was kinda of a struggle to finish it.

The first thing to do is to read the file `data.txt` with the `cat` command. It's a bunch of text of a hexdump. A hexdump is a datadump of a binary file to text showing more information about the file, probably made with the `xxd` command.

The good thing is that the file can be reversed to the original binary using the `xxd` command with the `-r` option:

```
-r | -revert
    reverse operation: convert (or patch) hexdump into binary.
```

But we are not doing this inside the home folder, we are doing many file management, so we are going to create a temp folder and play inside it. So we are going to use the command `mktemp -d` to create a temporary folder, and enter it with `cd`.

Then, we copy the file from home to our temporary directory. We'll use the `cp` command to copy the file from folder to the new directory:

```shell
cp ~/data.txt /tmp/temporary-folder/
```

And then, we can revert the hexdump to a binary file with `xxd -r`:

```shell
xxd -r data.txt data.bin
```

### Sisyphus Decompression

From now we will have a checklist to repeat until we have our password:

1. `file` : to identify the type of compression the file is
	- If the file type is gzip, rename the file to end with .gz with the command `mv data.bin data.gz`
2. extract the file with the correct file type:
	- gzip : use the `gunzip` command to extract the gzip file
	- tar : use the `tar -xvf` command to extract the tar file
	- bzip2 : use the `bzip2 -d` command to extract the bzip2 file
3. repeat

Repeating for some time, you will find a file that is an ASCII with the password for the next level.

