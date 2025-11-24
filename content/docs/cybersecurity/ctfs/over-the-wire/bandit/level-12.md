---
title: "Level 12"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 12

## Level Goal
> The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

## Commands you may need to solve this level
> grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

## Steps

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
