---
title: "Level 01"
# weight: 0
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Leviathan - Level 01

This challenge have 2 ways to solve it, one is a random way (the way I solved first) and a not random way. Let's check both of them.

## Steps

After connecting with the leviathan1 user using `ssh`, we have the home directory with the executable file named "check". So first we execute it to see what it does:

```shell
./check
```

This program ask us for a password, if the password is wrong it returns with a "goodbye" text, closing the program. If the password is correct, it returns something that we do not know yet. So, let's check how we can solve this.

### A Random Way

Using the command `strings` to show all the strings in the program, we can see that nothing calls our attention. Some possible words here and there, but nothing promising.

Then, we can use the `xxd` to show a hexdump with values side-by-side. And one thing that called my attention is that some words appear in the hexdump, but not in the `strings` command, one of them is the word "sex". Ok, weird, but there's nothing to lose trying it. So, running `./check` and inputting the word "sex" actually return us to a shell, the password is correct.

### A Not Random Way

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
