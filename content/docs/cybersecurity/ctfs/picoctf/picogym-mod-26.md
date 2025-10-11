---
title: "PicoGym: Mod 26"
weight: 2
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# PicoGym: Mod 26

This is a challenge from the [picoCTF picoGym](https://play.picoctf.org/practice/challenge/144?category=2&page=1), the challenge description follows:

```
"Cryptography can be easy, do you know what ROT13 is? 
cvpbPGS{arkg_gvzr_V'yy_gel_2_ebhaqf_bs_ebg13_uJdSftmh}"
```

This is pretty straightforward, we can use a website to decrypt the ROT13 encrypted message.

But in this case, I used a bash script to do this for me. Here's the script:

```bash
# Tring to find a flag using ROT13
# Where $1 is cvpbPGS{arkg_gvzr_V'yy_gel_2_ebhaqf_bs_ebg13_uJdSftmh}

echo "== ROT13 =="
echo ""
output=$(echo "$1" | tr 'N-ZA-Mn-za-m' 'A-Za-z')
echo "${output}"
```

The `tr` command is a substitution command, in this case, we will make the A to be N, B to be O and so on. This is the meaning of ROT13, it will shift each char 13 places:

![Image description](/images/picogym-mod-26/1.jpg)

It's bidirectional, so no worries of the ordering.

Running the script, the flag is revealed:

{{% details title="Answer: Flag" open=false %}}
```
picoCTF{next_time_I'll_try_2_rounds_of_rot13_hWqFsgzu}
```
{{% /details %}}
