---
title: "Leviathan"
# weight: 0
# bookFlatSection: false
# bookToc: true
# bookHidden: true
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Leviathan

Hello and welcome to my journey through Over The Wire, one of my favorites CTFs. Now tackling the Leviathan section, dedicated to the first steps of reverse engineering.

Mind that this documentation is for me only, if you didn't solve this ctf by yourself, do not read this posts, just go and solve these challenges for yourself. Do not spoil your experience.

---

## Level 00 -> Level 01

### Steps

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

---

## Level 01 -> Level 02

This challenge have 2 ways to solve it, one is a random way (the way I solved first) and a not random way. Let's check both of them.

### Steps

After connecting with the leviathan1 user using `ssh`, we have the home directory with the executable file named "check". So first we execute it to see what it does:

```shell
./check
```

This program ask us for a password, if the password is wrong it returns with a "goodbye" text, closing the program. If the password is correct, it returns something that we do not know yet. So, let's check how we can solve this.

#### A Random Way

Using the command `strings` to show all the strings in the program, we can see that nothing calls our attention. Some possible words here and there, but nothing promising.

Then, we can use the `xxd` to show a hexdump with values side-by-side. And one thing that called my attention is that some words appear in the hexdump, but not in the `strings` command, one of them is the word "sex". Ok, weird, but there's nothing to lose trying it. So, running `./check` and inputting the word "sex" actually return us to a shell, the password is correct.

#### A Not Random Way

Before going to the next section, let's talk about other way to achieve the same result, but not using all our luck. We are going to use the command `ltrace`. This command trace the library calls from the program to the system, let's check it using:

```shell
ltrace ./check
```

Some weird code will appear, but it's working and tracing for library calls. We input some random password input, like "asdf", and then press enter. The program will finish but keeping some tracing behind, and one of them that really calls our attention is the `strcmp()` (String Comparison) function:

```
strcmp("asdf", "sex")
```

The function called is trying to compare the string "asdf" with the string "sex". They are not equal, so the program recognizes as a wrong password.

With this, we can write the correct password in the program. Writing "sex" in the password field, we are redirected to a shell. Checking who is the user logged in with `whoami` command, the use "leviathan2" is returned, so we are logged as the leviathan2 user.

So, let's go grab the password for the next level at `/etc/leviathan_pass/leviathan2` path.

```shell
cat /etc/leviathan_pass/leviathan2
```

And now, we have the password for the next level.

---

## Level 02 -> Level 03

This challenge teach us about the dangers of redirecting a program execution of a system from one user to another, in this case the `cat` command.

### Steps

After connecting with the user leviathan2 using `ssh`, we land in the home directory with an executable file named "printfile". If we execute the file, we can see that it requires an argument, a file to be read. So after applying any file into the argument, we can see that it behaves the same as the `cat` command.

Let's check some details using the command `strings` to check any string useful for us:

```shell
strings ~/printfile
```

And the output show to us that it does not behave similar to the `cat` command. It **is** the `cat` command as a redirection:

```
Usage: %s filename
You cant have that file ...
/bin/cat %s
```

This gives us some ideas, one of them is to inject our code where the string argument (%s) is expected, making it execute two commands in one. So we are going to create some files in a temporary directory using `mktemp -d` and entering the created directory. Now we can create a dummy file to test and give the right permissions to the leviathan3 user.

```shell
touch "test"
chmod 777 "test"
~/printfile "./test"
```

If the permissions is available to the "leviathan3" user, we can see that it returns no error. So, we are going to combine two commands in one using the file name, making the program pretend that the filename is a command. First we are going to test with the `whoami` command, to see if it's everything running as expected:

```shell
mv "test" "test && whoami"
~/printfile "./test && whoami"
```

It returns the user "leviathan3", great, we are running some commands as the user leviathan3. Now, let's try accessing the bash of leviathan3 using the `bash` command:

```shell
mv "test && whoami" "test && bash"
~/printfile "./test && bash"
```

Now we are in the leviathan3 user terminal. Just access the file `/etc/leviathan_pass/leviathan3` to get the next level password.

---

## Level 03 -> Level 04

Have I told you about the difficulty progression of the Leviathan challenges before? It's very weird, we are going to do in this challenge the same we did before. 

### Steps

After we connect with the leviathan3 user, we can see that there's a file in the home directory called "level3". This is an executable file, the same as before. Let's check what it does running this application.

```
Enter the password> test
bzzzzzzzzzzap. WRONG
```

The program asks for a password, I wrote a simple "test" to see what happens, and it returns with a "wrong" text. So, we need to find the program's password to find our next level password.

Using the `strings` command to check for some secret string, we can see that there's many library calls. But no interesting possible password. Let's use the `ltrace` command as before:

```shell
ltrace ~/level3
```

Using the ltrace we can see that it compares some strings using `strcmp()` function. One of them is the string "h0no33" with "kakaka", which we do not have any way to change it, so let's ignore for now. Then, it asks for a password, we put "test" again to see what happens and other comparison appears as result:

```
strcmp("test\n", "snlprintf\n")
```

In this case, the character "\n" means a new line, that means a character to identify that we pressed enter. Removing the new line character, we are comparing the string "test" with "snlprintf", which means that the password is "snlprintf".

If we input the "snlprintf" into the `level3` program, it redirect us to a bash as the user leviathan4. We can check this using the `whoami` command:

```shell
$ whoami
leviathan4
```

Now, we just need to read the password at `/etc/leviathan_pass/leviathan4` to get the password for the next level.

---

## Level 04 -> Level 05

### Steps

After connecting to leviathan4, we can see in the content of the home directory with `ls -la` that we have a hidden directory called ".trash" and inside it there's an executable file named "bin"

Executing the "bin" file returns with a binary text. 

This one is very simple, just put the binary into some "binary to text converter" online or using cyberchef and the password for the next level will be revealed. 

---

## Level 05 -> Level 06

### Steps

This time the file we need to exploit is named "leviathan5", that can be found in the home directory.

Running the program `~/leviathan5` results in an error with the text:

```
Cannot find /tmp/file.log
```

This means that it looks for a file called "file.log" in the "/tmp/" folder. Ok, let's create the file with `touch` command:

```shell
touch /tmp/file.log
```

After creating the "file.log" and running the "leviathan5" program again, the same error happens. That's because the "file.log" is being constantly erased, not being able to keep the information in the /tmp/ folder.

So let's make the file and read it in the same line. We are going to `echo` a test text, so we can see if anything changes.

```shell
echo "test" > /tmp/file.log && ~/leviathan5
```

This time, no error is presented, actually, it returns the same text we added to the file.log: "test".

So, the program just reproduce the `echo` command as another user. Let's make a link from the "/etc/leviathan_pass/leviathan6" to "/tmp/file.log", making the files to be the same, but read differently.

```shell
ln -s /etc/leviathan_pass/leviathan6 /tmp/file.log && ~/leviathan5
```

Success! With that, the content of /tmp/file.log is the same as the leviathan6 password file. We have the password for the next level.

---

## Level 06 -> Level 07

### Steps

After connecting to the leviathan6 user, we can find an executable file named "leviathan6", this file requires a password with 4 digits, which we do not know yet.

The command `ltrace` and `strace` does not return with any possible numbers to try, so, this time, we are going to make a script to bruteforce the password.

Because it's only 4 digits, this is possible and will be quick. So, let's create a temporary directory with `mktemp` and create a new shell script called `bruteforce.sh` in that folder. Also, don't forget to make `chmod +x ./bruteforce.sh` to make the script executable.

The script will look something like this.

```bash
#!/bin/bash

for i in {0000..9999}
do
	~/leviathan6 $i
done
```

After running for some seconds, we have the access to a new terminal, using the `whoami` command, we can see that we are logged as leviathan7. Just open the "/etc/leviathan_pass/leviathan7" with the `cat` command, and we have our final password.

---
