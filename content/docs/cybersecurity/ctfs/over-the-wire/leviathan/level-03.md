---
title: "Level 03"
# weight: 0
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Leviathan - Level 03

Have I told you about the difficulty progression of the Leviathan challenges before? It's very weird, we are going to do in this challenge the same we did before. 

## Steps

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
