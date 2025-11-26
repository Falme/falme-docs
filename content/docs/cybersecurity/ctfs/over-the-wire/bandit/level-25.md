---
title: "Level 25"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 25

## Level Goal
> A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.
> You do not need to create new connections each time

## Commands you may need to solve this level
> 

## Steps

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


