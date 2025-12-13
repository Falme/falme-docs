---
title: "Bandit"
# weight: 0
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit

Hello and welcome to my journey through Over The Wire, one of my favorites CTFs.

I've already completed this challenge years ago, but I decided to return to it 
to revisit it, and this time writing about the solutions.

Mind that this documentation is for me only, if you didn't solve this ctf by 
yourself, do not read this posts, just go and solve these challenges for 
yourself. Do not spoil your experience.

---

## Level 00

> The goal of this level is for you to log into the game using SSH. The host to which you need to connect is bandit.labs.overthewire.org, on port 2220. The username is bandit0 and the password is bandit0. Once logged in, go to the Level 1 page to find out how to beat Level 1.

This level is an introduction to the system that we will dive in: Bandit.

So the solution is present in the problem, it says which are the login and 
password to login into the game. The only problem is how to use the SSH 
command.

In this case, it will be something like:

```shell
ssh bandit.labs.overthewire.org -p 2220 -l bandit0
```

Where:
- `ssh` : Is the SSH command call 
- `bandit.labs.overthewire.org` : Is the host to connect
- `-p 2220` : Is the door to connect, in the case of bandit, is always 2220
- `-l bandit0` : Is the username that is connecting, in this case, bandit0

So, with that, we finished level0. Now we can go to Level1.

---

## Level 00 -> Level 01

From now on, the challenge will begin, normally I use a spoiler section for the 
flag answer, but in the case of the OverTheWire, the password change 
occasionally, so I will only make a step-by-step guide with no final answer.

### Level Goal
> The password for the next level is stored in a file called readme located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

### Commands you may need to solve this level
> ls , cd , cat , file , du , find


### Steps

This level has the intention to teach you the basics of using a unix terminal 
or system. In my case I'm using a Linux in a Virtual Machine.

On the last level (level 0) we gained access to a user named bandit0. We can 
see this at the shell identification for the user currently logged in:

```shell
bandit0@bandit:~$
```

Reading the goal of this challenge, our objective is in a file named `readme`. 
So we first list all the files in the current folder with `ls`. It shows only a 
file named `readme`. That's the file containing the answer.

To read the content of the file we will use a command called `cat`, this will 
show it's content.

With that we have a text with the password for the next level.

---

## Level 01 -> Level 02

### Level Goal
> The password for the next level is stored in a file called - located in the home directory

### Commands you may need to solve this level
> ls , cd , cat , file , du , find

### Steps

In this case the file is literally called `-`, you can list the file to be 
shown using the command `ls` and you can see the file is there.

But it's not possible to just use the command `cat -` to read the file. See, 
the dash character is a special character, it's used to define options into a 
program. Let's see another example:

```shell
ls -l 
```

In this case, the command `ls` is using an option `-l`, which list files in a 
folder in a long list format.

So we cannot use `cat -`, because cat is selecting an option to be executed. To 
make this work we will need an alternative (and the reason of this challenge), 
because cat is waiting an input of a file, we will use some I/O redirection.

First let's explain the Input/Output redirection. If we make something like 
`echo "test text" > file.txt`, this means that we will write into a console 
using `echo` the text `"test text"`, and then redirect the Output into a file 
named `file.txt` using the `>` signal.

Ok, if the `>` means output, the `<` means input, right?

So let's Input the file `-` into the `cat` command, it will be something like:

```shell
cat < -
```

And with that, we have the password for the next level.

---

## Level 02 -> Level 03

### Level Goal
> The password for the next level is stored in a file called `--spaces in this filename--` located in the home directory

### Commands you may need to solve this level
> ls , cd , cat , file , du , find

### Steps

This one is similar to the previous level (level 2), but in this case the 
special character is the space character. Normally the space after calling a 
program is a separator of arguments. So, let's use an example with `cat`:

```shell
cat file.txt
```

So in this case we have the program `cat` and an argument expected separated by 
a space, a file named `file.txt`.

Reading a file with space in the name cause confusion to the console, it can't 
separate what is an space and what is an argument. To make it work we, 
surprisingly, have two ways to read the file.

### Quotes

We can use quotes or double quotes around the path, so space is inside quotes 
and arguments is inside the quotes. It will be something like:

```shell
cat < "--spaces in this filename--"
```

(The `<` is necessary, since `-` is also a special character)

### Backslash

Backslash is also an special character, but it's used to "Escape a Character", 
making a space be read as a space and not a separator. It will be something 
like:

```shell
cat < --spaces\ in\ this\ filename--
```

So every space in the path should be replaced with backslash+space. 
With this, the answer to the next level is revealed.

---

## Level 03 -> Level 04

### Level Goal
> The password for the next level is stored in a hidden file in the `inhere` directory.

### Commands you may need to solve this level
> ls , cd , cat , file , du , find

### Steps

This challenge teaches us about hidden files and directories. In Linux, hidden 
files is marked with a dot (`.`) before the file name. This makes it marked as 
hidden and will not show in a common `ls` command.

So if we use ls in the home directory, we can find the directory `inhere`. Then 
we enter the `ìnhere` directory using the `cd` command:

```shell
cd inhere
```

Now, inside `inhere` directory, if the list the files with `ls`, no file can be 
found. To show hidden files inside a directory we will use the option `-a` to 
show all files, including hidden files. It will be something like:

```shell
ls -a
```

With that, the file `...Hiding-From-You` is revealed, reading the file with `cat`:

```shell
cat < ...Hiding-From-You
```

Reveals the password for the next level.

---

## Level 04 -> Level 05

### Level Goal
> The password for the next level is stored in the only human-readable file in the `inhere` directory. Tip: if your terminal is messed up, try the “reset” command.

### Commands you may need to solve this level
> ls , cd , cat , file , du , find

### Steps

(I know what happens in the next level, so we gonna play simple in this one)

The goal says that there's a human-readable file in the `inhere` directory with 
the password for the next level. This means that the file is not a file in 
machine code language. 

Machine code file is kinda garbage when you try to read it, so we will try to 
find a simple text file. First we enter the `inhere` folder with `cd`:

```shell
cd inhere
```

Listing the files in the folder with `ls` we have a list of files:

```
-file00
-file01
-file02
-file03
-file04
-file05
-file06
-file07
-file08
-file09
```

This time, we will use a command called `file`, this shows the details and 
information of that file. The problem here is that `file < -file00` does not 
work, this time we need to use another way to show the console we don't want 
to call an option, we will use `--`:

```shell
file -- -file00
```

So using the command `file` in each one of the files we have the following 
information:

```
-file00: data
-file01: OpenPGP Public Key
-file02: OpenPGP Public Key
-file03: data
-file04: data
-file05: data
-file06: data
-file07: ASCII text
-file08: data
-file09: data
```

The information we gathered follows:

- data : machine code, not human-readable
- OpenPGP Public Key : key for encryption, not human-readable
- ASCII text : character encoding standard, human-readable

With that, we have the file we want `-file07`, we read it with cat:

```shell
cat < -file07
```

Resulting with the password for the next level.

---

## Level 05 -> Level 06

### Level Goal
> The password for the next level is stored in a file somewhere under the `inhere` directory and has all of the following properties:
>
>    human-readable  
>    1033 bytes in size  
>    not executable  

### Commands you may need to solve this level
> ls , cd , cat , file , du , find

### Steps

This time we are going to use the power of Linux and the `find` command. For this challenge, I recommend you to read the manual for `find` command, using `man find`, this will be very useful.

We are looking for a file that is human-readable, 1033 bytes and non-executable. For this, we will use `find` with some filtering parameters. But first, to play with it, we will just list all files, no need to open and enter the `inhere` folder:

```shell
find ./
```

This command will list all files in the current folder (that `./` means current folder) in a recursively way. What we need now is to filter the results to be the exact parameters we are looking for. Checking the manual for `find` we can find the `-executable` option:

```
-executable
              Matches files which are executable and directories which
              are searchable (in a file name resolution sense) by the
              current user.
```

Ok, now we can filter the results to be only the executable files, but that's not what we are looking for, we are looking for files that are NOT executable. Returning to the manual, we can find the `-not` option, that inverts the result of an option:

```
-not expr
              Same as ! expr, but not POSIX compliant.
```

Making use of these options, we can list all the non-executable files inside `inhere`:

```shell
find ./ -not -executable
```

But still, there's too many files to be searching by hand, we need to filter better. This time we will filter by size, looking for files that have exactly 1033 bytes. Checking the manual for `find` again, we can find the `-size` option:

```
-size n[cwbkMG]
              File uses less than, more than or exactly n units of space,
              rounding up.  The following suffixes can be used:

              `c'    for bytes
```

With the size option, we can add it to our command and check for the results:

```shell
find ./ -not -executable -size 1033c
```

With this command, the result is filtered to only one file:

```
./inhere/maybehere07/.file2
```

Reading the file with the `cat` command, the password for the next level is revealed.

---

## Level 06 -> Level 07

### Level Goal
> The password for the next level is stored somewhere on the server and has all of the following properties:
>
>    owned by user bandit7  
>    owned by group bandit6  
>    33 bytes in size  

### Commands you may need to solve this level
> ls , cd , cat , file , du , find , grep

### Steps

This challenge is a next step from the last level (level 6), the solution is practically the same as before, but some different parameters.

First of all, the goal is asking to search a file somewhere in the server, that means we are not limited to the `inhere` or home directory. So, we are looking through the files in `/`, that is the root directory, from that, we can recursively find all possible files in the system.

So, let's start with the options in the `find` command we already know, that is the `-size` option:

```shell
find / -size 33c
```

This command will look through all the system trying to find a file with 33 bytes in size. The results show too much files, so we need to filter better to find the correct file. Looking through the manual, we can find the `-user` option:

```
-user uname
              File is owned by user uname (numeric user ID allowed).
```

This command permit us to filter the files owned by bandit7 user.

```shell
find / -size 33c -user bandit7
```

Still showing too many possible files, so we need to filter by group as well. Looking through the manual of `find`, we can find the `-group` option:

```
-group gname
              File belongs to group gname (numeric group ID allowed).
```

This command permit us to filter the files in the bandit6 group.

```shell
find / -size 33c -user bandit7 -group bandit6
```

With this you can already find the file with the password for the next level. But, there's too many files showing in the results that we don't have permission to access. So we are doing some cleaning here, so only the correct file will show to us.

#### File Descriptors

We already learned about redirection with `<` and `>` special characters, know we will learn about File Descriptors. The different type of File Descriptors are:

- stdin: Represented by the number 0, is the Input
- stdout: Represented by the number 1, is the common print output
- stderr: Represented by the number 2, is the error print output

In our case, we want to remove the Permission errors in the output of our `find` command. So we are using the stderr (2) in the example.

Going through the command before, we are adding the following redirection:

```shell
2>/dev/null
```

Explaining a bit the structure:

- `2` : is the stderr, or error, that the command will output, like a tag from the output print
- `>` : is the redirection, we want the error messages to go elsewhere, anywhere but our console
- `/dev/null` : is the path to a special file, used for discarding informations, we want to discard the errors in this case

And with that we discarded the error messages. In the end the final command will be something like:

```shell
find / -size 33c -user bandit7 -group bandit6 2>/dev/null
```

With the result the correct file is revealed, using `cat` to read the file shows the password for the next level.

---

## Level 07 -> Level 08

### Level Goal
> The password for the next level is stored in the file data.txt next to the word millionth

### Commands you may need to solve this level
> man, grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

### Steps

This challenge have the intention to teach us the `grep` command. `grep` print lines with a match pattern, in this example, a string.

Imagine you are looking through a big file and you need a specific word, grep will ignore other lines and only shows you the one you need.

Why this is necessary, you say? First of all, it's practical. Second, you can automate some searches from command line, but third and most important to me, to find patterns in huge files.

Some files, like lists and dictionaries, is just a text file (.txt) with gigabytes in size. If you try to open it with notepad it's not gonna work. So, to play with it, let's go back to the Level Goal.

We want to find the word "millionth", so using `grep`, it's something like:

```shell
cat data.txt | grep 'millionth'
```

Where:
- `cat` : opens the file
- `data.txt` : the file to read the data
- `|` : pipe special character, where it send one command output to another command input
- `grep` : command to filter the output of text from `cat data.txt`
- "millionth" : the string we want to filter

With this, the password for the next level will appear next to the word millionth.

---

## Level 08 -> Level 09

### Level Goal
> The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

### Commands you may need to solve this level
> grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

### Steps

We need to find an unique line in the data.txt file that does not repeat, because this file is filled with possibilities for a password. The first instinct is to use the `uniq` command, because, in it's description:

```
uniq - report or omit repeated lines
```

And it's correct to use the `uniq` command, that only shows lines that are unique and not repeated. But, in the end of the `uniq` manual page, we have:

```
'uniq' does not detect repeated lines unless they are adjacent.
       You may want to sort the input first, or use 'sort -u' without
       'uniq'.
```

So, before we use the `uniq` command to filter the repeated lines, we need to sort the lines alphabetically, and then use the `uniq` command. So the command will be something like:

```shell
sort data.txt | uniq -u
```

Where:
- `sort` : sort the lines alphabetically
- `data.txt` : file to be sorted and processed
- `|` : pipe special character, to send the sorted content to the `uniq` command
- `uniq` : command to report or omit repeated lines
- `-u` : option of `uniq` to only print unique lines

With that, only the password to the next level will be printed.

---

## Level 09 -> Level 10

### Level Goal
> The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several '=' characters.

### Commands you may need to solve this level
> grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

### Steps

Most of the file `data.txt` is filled with machine code, this means that is not human-readable, we could use the command `xxd` to read the file and find manually for the password, but there's too many content, and will take us too much time.

So, we are going to think this way, we only need the readable part of the file, in another words, only the strings in the file. So we are going to use the `strings` command, where it shows only the strings inside a file and nothing more. Making something like:

```shell
strings data.txt
```

Will show only the strings in the file. But it's still too many content. In the Goal text it says that the password is preceded by several '=' characters, so we can use the `grep` command to filter the several '=' characters. But how many '=' is several? Let's try 2, looks enough for the search. It looks something like:

```shell
strings data.txt | grep '=='
```

And with that, the password for the next level is revealed.

---

## Level 10 -> Level 11

### Level Goal
> The password for the next level is stored in the file data.txt, which contains base64 encoded data

### Commands you may need to solve this level
> grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

### Steps

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

---

## Level 11 -> Level 12

### Level Goal
> The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

### Commands you may need to solve this level
> grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

### Steps

This challenge is similar to the previous one (level 11), but this time we are not dealing with the base64 encryption, we are dealing with the ROT13 encryption. 

The ROT13 encryption is pretty simple, it moves a character 13 letters from the destiny, just like a rotation. If the character is 'A', it will be translated to 'N', if the character is 'B', it will be translated to 'O', and so on. To demonstrate, check the image below:

![Image description](/images/picogym-mod-26/1.jpg)

For our challenge we will use a command called `tr`, the command translate the characters from one character to another. With some examples it will be very simple to understand. We are going to translate a character in the word `test`:

```shell
echo "test" | tr "s" "N"
```

In the case above, we are using `tr` to change the 's' character to a 'N' character. The result will be `teNt`, with N in uppercase, just like we wrote. But it's not only a single character that can be translated, but also a sequence of characters:

```shell
echo "test" | tr "r-u" "N"
```

In the case above, we are translating the characters through 'r' to 'u' (inclusive) to a 'N' character. The result will be `NeNN` because 't' and 's' are between 'r' and 'u' in the alphabet.

Now you might be wondering, if both of them have a ranged argument. Let's do this:

```shell
echo "test" | tr "r-u" "N-Q"
```

In the case above, we are translating the characters [r,s,t,u] to the [N,O,P,Q] characters. The result will be `PeOP`, because they will try to match the index position of both arrays to find the result. The 't' is at the same position as 'P' and 's' is at the same position as 'O'.

So, now we can make some changes to finish our challenge, we will do the following command:

```shell
cat data.txt | tr "N-ZA-Mn-za-m" "A-Za-z"
```

Where:
- `cat data.txt` : will read the encoded ROT13 file
- `|` : will pipe the `data.txt` result to the `tr` command
- `tr` : will translate the encoded characters to the decoded characters
- `"N-ZA-Mn-za-m"` : is the original characters we want to change
	- The array for the characters will look something like:
	- [N, O, P, Q, ..., Y, Z, A, B, C, ..., L, M, n, o, p, q, ..., y, z, a, b, c, ..., l, m]
	- In another words, a dislocated alphabet in uppercase and lowercase
- `"A-Za-z"` : is the transformation of characters by index, so it will look something like:
	- [A, B, C, ..., Y, Z, a, b, c, ..., y, z]

The first Array of characters will be replaced by the second array of characters, making a successful ROT13 conversion back. Now you have the password for the next level.

---

## Level 12 -> Level 13

### Level Goal
> The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the command “mktemp -d”. Then copy the datafile using cp, and rename it using mv (read the manpages!)

### Commands you may need to solve this level
> grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd, mkdir, cp, mv, file

### Steps

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

#### Sisyphus Decompression

From now we will have a checklist to repeat until we have our password:

1. `file` : to identify the type of compression the file is
	- If the file type is gzip, rename the file to end with .gz with the command `mv data.bin data.gz`
2. extract the file with the correct file type:
	- gzip : use the `gunzip` command to extract the gzip file
	- tar : use the `tar -xvf` command to extract the tar file
	- bzip2 : use the `bzip2 -d` command to extract the bzip2 file
3. repeat

Repeating for some time, you will find a file that is an ASCII with the password for the next level.

---

## Level 13 -> Level 14

### Level Goal
> The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Look at the commands that logged you into previous bandit levels, and find out how to use the key for this level.

### Commands you may need to solve this level
> ssh, scp, umask, chmod, cat, nc, install

### Steps

Normally we access a level using the `ssh` command with a password request, but this time we are using a private key. A secure connection using public and private keys are beyond the scope of this writeup, but I can assure that a private key should not be accessible for other users. This is the catch of this challenge, the private key is accessible and we are going to use it to access the next level.

At the home repository we can find the file `sshkey.private`. This is a RSA Private Key, and we can read it's content with the `cat` command, resulting in a bunch of encoded verification. We are going to copy this private key and save into our machine in a new file called `id_rsa`.

Now outside of the bandit user, we are going to connect to the next level with only the rsa key. The first place that the `ssh` verify for keys is at the path `~/.ssh`, so we are going to put our stolen private key there.

After that, just connect through `ssh` to the next level, so no password is necessary.

#### WARNING: UNPROTECTED PRIVATE KEY FILE!

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

---

## Level 14 -> Level 15

### Level Goal
> The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

### Commands you may need to solve this level
> ssh, telnet, nc, openssl, s_client, nmap

### Steps

Understanding the goal text, the port 30000 is being listened, waiting for an input of the current level password. To make a connection TCP/UDP through a listening port, we will use the command `nc`. The command `nc` (or netcat) is made for this kind of work, so, we are going to make a connection with:

```shell
nc localhost 30000
```

No answer will be displayed in the console, but it's waiting for the input. Just write the password of the current level, press enter and the password for the next level will be shown.

---

## Level 15 -> Level 16

### Level Goal
> The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL/TLS encryption.
> 
> Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.

### Commands you may need to solve this level
> ssh, telnet, nc, ncat, socat, openssl, s_client, nmap, netstat, ss

### Steps

This challenge is very similar to the previous one (level 15), but this time we need to make a secure connection using SSL/TLS encryption. For that, we need a more powerful Netcat, so we are going to use the `ncat` command with the option `--ssl`:

```
--ssl                  Connect or listen with SSL
```

Making the secure connection to the localhost at port 30001, like this:

```shell
ncat --ssl localhost 30001
```

And after making the connection, we put the current level password and press enter. With that, the password for the next level is revealed.

---

## Level 16 -> Level 17

### Level Goal
> The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL/TLS and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.
> 
> Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.

### Commands you may need to solve this level
> ssh, telnet, nc, ncat, socat, openssl, s_client, nmap, netstat, ss

### Steps

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

---

## Level 17 -> Level 18

### Level Goal
> There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new
> 
> NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19

### Commands you may need to solve this level
> cat, grep, ls, diff

### Steps

This challenge is meant to teach us about the `diff` command. The `diff` compares two files and output us the difference between them, very commonly used with `git` and others version control programs. The idea is that there's an old file and a new file, it will show you what changed between those 2.

The character "<" is a difference that exited the file, and the character ">" is a difference that entered the file. So, let's compare the files:

```shell
diff passwords.old passwords.new
```

The password for the next level should be next to the ">" character.

---

## Level 18 -> Level 19

### Level Goal
> The password for the next level is stored in a file readme in the home directory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

### Commands you may need to solve this level
> ssh, ls, cat

### Steps

This challenge kick you out just after you connect with the bandit18 user, the .bashrc initializes a script that prints the text "Byebye!" and disconnects you. 

The way to solve this is to inject a command through ssh before it kicks you. Fortunately, the `ssh` command accepts a injected command inside the connection sequence. It's something like this:

```shell
ssh bandit.labs.overthewire.org -p 2220 -l bandit18 "cat ~/readme"
```

The `"cat ~/readme"` is a common command for the console, and it will run just after the connection was completed. In the case we want to read the content of the file `readme`.

Just with that, the password for the next level is revealed.

---

## Level 19 -> Level 20

### Level Goal
> To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

### Commands you may need to solve this level
> 

### Steps

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

---

## Level 20 -> Level 21

### Level Goal
> There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).
> 
> NOTE: Try connecting to your own network daemon to see if it works as you think

### Commands you may need to solve this level
> ssh, nc, cat, bash, screen, tmux, Unix ‘job control’ (bg, fg, jobs, &, CTRL-Z, …)

### Steps

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

---

## Level 21 -> Level 22

### Level Goal
> A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

### Commands you may need to solve this level
> cron, crontab, crontab(5) (use “man 5 crontab” to access this)

### Steps

This time, we are going to the `cron` config files. `cron` is a scheduler, it runs in the background to do something from time to time. The tasks can be found in the path /etc/cron.d/, so we are going to take a look there.

We can find some files, but at the moment the file `cronjob_bandit22` is the best to our goal. Using the `cat` command to read the file, we can find the next path:

```
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

We know that /dev/null is a special discard file, but something may be in the /usr/bin/cronjob_bandit22.sh script that help us. So, we use `cat` again to check it out what it does.

It writes from time to time the password from /etc/bandit_pass/bandit22 into a file in the /tmp/ folder. Opening this file in the /tmp/ folder, the password for the next level is revealed.

---

## Level 22 -> Level 23

### Level Goal
> A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.
> 
> NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

### Commands you may need to solve this level
> cron, crontab, crontab(5) (use “man 5 crontab” to access this)

### Steps

The steps to reach the script that is running in the `cron` routine is the same as the last level (level 22) :

- Open the /etc/cron.d/ folder with `cd`
- Read the file content of `cronjob_bandit23`
- Go to path of the script inside it (`cronjob_bandit23.sh`)
- Read the script

This is a pretty simple script, it takes the current user logged in with `whoami` command, encode the result with a `md5sum` encoder and trim the unnecessary characters, the result of this encoding is the name of the file in the /tmp/ folder that contains the password for the next level.

How to find the result is just a case of repeating the script steps correctly until you reach the correct file path, it's something like:

```shell 
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
```

Where
- `echo I am user bandit23` : print a string to be redirected to `md5sum` and bandit23 is the pretended user
- `md5sum` : encode in MD5 the last string
- `cut -d ' ' -f 1` : removes the extra unnecessary characters

Reading the file in the found path, the next level password is revealed.

---

## Level 23 -> Level 24

### Level Goal
> A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.
> 
> NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!
> 
> NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

### Commands you may need to solve this level
> chmod, cron, crontab, crontab(5) (use “man 5 crontab” to access this)

### Steps

The way to find the script inside the cron.d folder is the same as the last 2 levels (level 22 and level 23). After you find the script `cronjob_bandit24.sh` analyze it, if you didn't understand a part of it, check the following commented code block:

```shell 
#!/bin/bash # Run as a bash script

myname=$(whoami) # save into the variable "myname" the current user

cd /var/spool/$myname/foo # Enter in a directory called "foo"
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*; # Loop through all the files in the folder
do
    if [ "$i" != "." -a "$i" != ".." ]; # If it's not a directory
    then
        echo "Handling $i" # Current script to handle
        owner="$(stat --format "%U" ./$i)" # Get name of the owner of the script
        if [ "${owner}" = "bandit23" ]; then # If owner is the bandit23
            timeout -s 9 60 ./$i # run a script and hang if exceeds 1 minute
        fi
        rm -f ./$i # Remove the script
    fi
done
```

In another words, there's a script that runs from time to time that check inside the `/var/spool/bandit24/foo/` directory for scripts and run them if the owner is bandit23. After running the script, it deletes the script.

The way to solve this problem is to copy a self-made shell script to the `/var/spool/bandit24/foo/` directory that retrieves the password for the next level. So, first let's create a shell script called `getpass.sh` inside a temporary folder using `mktemp -d` command, you can edit the text in your favorite text editor (nano, vim, etc...).

The script I wrote is the following:

```shell 
#!/bin/bash

result=$(cat /etc/bandit_pass/bandit24)
echo "res" > /tmp/tmp.tempfolderhash/$result
```

Where:
- `#!/bin/bash` : tells the runner to use the bash
- `result=$(cat /etc/bandit_pass/bandit24)` : save the bandit24 password into a variable called "result"
- `echo "res" > /tmp/tmp.tempfolderhash/$result` : save a file in the temp folder with the password as the filename, just like in the last level script. ("res" is a discard argument for echo, you could use `touch`)

Now, change the permissions to be executable from other users using `chmod a+x getpass.sh`, and copy the script to the `/var/spool/bandit24/foo/` using the `cp` command:

```shell
cp ./getpass.sh /var/spool/bandit24/foo/getpass.sh
```

Now, just wait a little and the password for the next level will appear as a file in your created temporary directory.

---

## Level 24 -> Level 25

### Level Goal
> A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.
> You do not need to create new connections each time

### Commands you may need to solve this level
> 

### Steps

For this challenge, we are going to use `ncat` for the connection on port 30002 and a script to generate a text. The connection is pretty simple:

```shell
ncat localhost 30002
```

This connects to a waiting daemon where it expects the combination bandit24_password+pincode. But trying one by one will take an eternity. One good thing is that `ncat` can receive a textfile of inputs and process it normally. So, imagine that we have the following file `list.txt`:

```
password_bandit24 0
password_bandit24 1
password_bandit24 2
password_bandit24 3
```

If we input the file this way:

```shell
ncat localhost 30002 < .list.txt
```

Each line will be processed like a common input from the user. Well, ok, we just need to write 10000 lines like that. Also taking an eternity to do that. So we are going to create a shell script to do that for us. We are going to create a script called `create_list.sh`, make it executable with `chmod +x create_list.sh` and edit it with something like this:

```shell
for i in {0..10000}
do
echo "password_bandit24 $i" >> ./list.txt
done
```

Where:
- `for i in {0..10000}` : is a loop that repeats from 0 to 10000, saving the current position in `$i` variable
- `echo "password_bandit24 $i"` : print the string with the password+pincode from the variable
- `>> ./list.txt` : add a new line into a file, in this case the `list.txt` file on each loop interaction

Run this script with `./create_list.sh` and the file will have all the lines necessary to find the next password. Now to get the next password just run `ncat` with our filled list.

```shell
ncat localhost 30002 < .list.txt
```

---

## Level 25 -> Level 26

### Level Goal
> Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.
> 
>    NOTE: if you’re a Windows user and typically use Powershell to ssh into bandit: Powershell is known to cause issues with the intended solution to this level. You should use command prompt instead.

### Commands you may need to solve this level
> ssh, cat, more, vi, ls, id, pwd

### Steps

This one gave me problems the first time I solved it, you need to think outside of the box, or about the size of the box.

The problem is, in the home folder, there's a rsa private key to login into bandit26 user, but after the login it disconnects. We need to check other things first.

The goal text says that bandit26 is not using /bin/bash for the shell, so we need to check which shell it's using. For that we need to check the information for all the users in the system. Let's check the `passwd` file.

```shell 
cat /etc/passwd | grep bandit26
```

This file contains the path of a shell script named `showtext`, showing it contents with `cat` reveals the simplicity of the script. It just set an environment, print the presentation text on screen and exit the terminal.

The trick is on the line reading the presentation text. It uses the `more` command, and we will use that to our favor.

First, make the terminal height very small, like 2 or 3 lines in height, then connect normally to the bandit26 user. Half of the presentation text should appear, because the `more` command will pause the process of the script, waiting for the user input.

In this moment, press the `v` key, it will go to editing/visual mode for vim. Now you are in an text editor. We will now open the password for the bandit26 level. Type the following still in vim:

```shell 
:e /etc/bandit_pass/bandit26
```

This will open the file directly in the terminal. Copy the password for the next level and close the vim with `:qa` (quit all)

---

## Level 26 -> Level 27

### Level Goal
> Good job getting a shell! Now hurry and grab the password for bandit27!

### Commands you may need to solve this level
> ls

### Steps

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

---

## Level 27 -> Level 28

### Level Goal
> There is a git repository at ssh://bandit27-git@bandit.labs.overthewire.org/home/bandit27-git/repo via the port 2220. The password for the user bandit27-git is the same as for the user bandit27.
> 
> Clone the repository and find the password for the next level.

### Commands you may need to solve this level
> git

### Steps

From now on, we are going to play with `git`. Git is a version control program, it uses repositories to track changes in a project, creating a timeline of changes in a effective way and with great performance.

This is the basic of dealing with `git`, we are going to clone a repository into a folder, which is making a copy from a server into our machine. The command will be something like:

```bash
git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
```

Remember that we are dealing with a different port (2220), the common ssh port is 22, so we needed to add it to the ssh address.

After cloning the repository, find the file README, it contains the password for the next level.

---

## Level 28 -> Level 29

### Level Goal
> There is a git repository at ssh://bandit28-git@bandit.labs.overthewire.org/home/bandit28-git/repo via the port 2220. The password for the user bandit28-git is the same as for the user bandit28.
> 
> Clone the repository and find the password for the next level.

### Commands you may need to solve this level
> git

### Steps

Now, do the same as the last level (level 28) to clone the repository into a folder, but now for the bandit28-git.

After that, we can check that the file `README` does not contain the password for the next level. Remember that I said `git` have a timeline of changes in the project? Well, this is the time to check what was changed. Let's do the command:

```bash
git show
```

This command will show what was the changes in the file `README`, and also revealing the password for the next level.

---

## Level 29 -> Level 30

### Level Goal
> There is a git repository at ssh://bandit29-git@bandit.labs.overthewire.org/home/bandit29-git/repo via the port 2220. The password for the user bandit29-git is the same as for the user bandit29.
> 
> Clone the repository and find the password for the next level.

### Commands you may need to solve this level
> git

### Steps

Clone the repository just like in the previous level, and we are going to find the password for this challenge.

This time the challenge teach us about branches. In `git` branches are different timelines of a project, maybe you want to test a feature, so you make a new branch to test this feature in a non-destructive way. So, first we are going to check how many branches are available. We can check with:

```shell 
git branch --all
```

This will show all the branches available in the project. We are going to check the development branch called `origin/dev`, to change the branch to that one, we will use the `git checkout`:

```shell 
git checkout origin/dev 
```

Now, we changed from the branch master to origin/dev, opening the README.md file with `cat` now contains the password for the next level.

---

## Level 30 -> Level 31

### Level Goal
> There is a git repository at ssh://bandit30-git@bandit.labs.overthewire.org/home/bandit30-git/repo via the port 2220. The password for the user bandit30-git is the same as for the user bandit30.
> 
> Clone the repository and find the password for the next level.

### Commands you may need to solve this level
> git

### Steps

First of all, clone the repository just like in the previous levels. After that we can continue the journey to find the password for the next level.

This time the `git log` and `git show` have only one file, with no changes. This seems to be a wall, maybe there's a different branch, but no branch bring us to the password. 

Actually the answer is on the filesystem inside the hidden folder `.git`, it shows some packed files, so, let's check the refs for the packages in the repository. Open the `.git/packed-refs` files with `cat` to show a weird tag place:

```
843...351 /refs/tags/secret
```

The first hash is probably a commit hash on a tag called secret. So we are going to use a command called `git cherry-pick` to get the specific commit from the tree.

```
git cherry-pick 843...351
```

Then `git` return to us a message: "Can't Cherry-pick a blob". Ok, this means that we are not dealing with a commit, but with a blob of some kind. Reaching the `git` manual, we can find a command that deals with that for us called `unpack-file`, saying:

```
git-unpack-file(1)
           Creates a temporary file with a blob’s contents.
```

So we are going to do the following command:

```shell 
git unpack-file 843...351
```

After this, a file will be created in the same folder named `.merge_file_uEpLe8`, then use `cat` to read the unpacked file, revealing the password for the next level.

---

## Level 31 -> Level 32

### Level Goal
> There is a git repository at ssh://bandit31-git@bandit.labs.overthewire.org/home/bandit31-git/repo via the port 2220. The password for the user bandit31-git is the same as for the user bandit31.
> 
> Clone the repository and find the password for the next level.

### Commands you may need to solve this level
> git

### Steps

First of all, clone the repository just like in the previous levels. After that we can continue the journey to find the password for the next level.

In this challenge we are going to learn about the `.gitignore`, this is a file that dictates what's NOT being tracked in the project. Maybe a binary, a file you don't want to send to the repository, etc.

After cloning the repository, you can find the `.gitignore` and read it with `cat`, it shows that every "*.txt" file is being ignored, so no .txt file will be tracked.

Then, there's the README.md file, the text in it says:

```
This time your task is to push a file to the remote repository.

Details:
	File name: key.txt
	Content: 'May I come in?'
	Branch: master
```

So, we need to create a file named `key.txt` with the text inside as "May I come in?" on the master branch. But, the `.gitignore` does not track the .txt files, so we cannot push this file.

So, we are going to delete the .gitignore and commit our changes, without the .gitignore, there's nothing to deny us to send the .txt file.

So send a change in `git`, usually we go through this pipeline:

- `git add ./file` : We add the file to the stage, preparing the file to the commit
- `git commit -m "message"` : We commit our changes and files, usually we also write a message to know what changed
- `git push --all` : Then we push our changes and files to the server where the repository is located.

With the new file, it will be something like this:

```shell
git add ./key.txt
git commit -m "May I come in?"
git push --all
```

If you did everything right, `git` will throw an error with the password for the next level. 

---

## Level 32 -> Level 33

### Level Goal
> After all this git stuff, it’s time for another escape. Good luck!

### Commands you may need to solve this level
> sh, man

### Steps

This is the last challenge available at the moment, this one is another shell escapade and get the password inside the /etc/bandit_pass/ directory.

The shell that you are trapped in makes every command uppercase, which is a problem for linux, because the commands are case-sensitive, you cannot write any command without giving you an error. 

The solution that I found is to write `$0`, one of the special variables that runs the current shell, but in this case it redirects to another kind of shell, not locked in uppercase anymore. Inside this new shell, write `bash` to load the bash shell terminal.

Now, free from the uppercase terminal, we can go to `/etc/bandit_pass/bandit33` and read the next level password with `cat`.

---

