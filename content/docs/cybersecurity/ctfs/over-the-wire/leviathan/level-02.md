---
title: "Level 02"
# weight: 0
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Leviathan - Level 02

This challenge teach us about the dangers of redirecting a program execution of a system from one user to another, in this case the `cat` command.

## Steps

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
