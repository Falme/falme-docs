---
title: "PicoGym: Flag Hunters"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# PicoGym: Flag Hunters

> At the moment, PicoGym was moved to CyLabAcademy, some links may differ.

This challenge is called "Flag Hunters", we will analyze a python code, understand some flaws and find a flag in the program connected through netcat `nc`.

- Difficulty: Easy
- Category: Reverse Engineering
- Language: Python 3.x

> Lyrics jump from verses to the refrain kind of like a subroutine call. There's a hidden refrain this program doesn't print by default. Can you get it to print it? There might be something in it for you.  
>  
> Connect to the program with netcat:  
>  
> `$ nc verbal-sleep.picoctf.net 69420`

(The connection address and port may differ from yours)


## How to Solve

First, let's open the source code `lyric-reader.py`:

```python
import re
import time

# Read in flag from file
flag = open('flag.txt', 'r').read()

secret_intro = \
'''Pico warriors rising, puzzles laid bare,
Solving each challenge with precision and flair.
With unity and skill, flags we deliver,
The ether’s ours to conquer, '''\
+ flag + '\n'

song_flag_hunters = secret_intro +\
'''

[REFRAIN]
We’re flag hunters in the ether, lighting up the grid,
[...]
We’re chasing that victory, and we’ll never quit.
CROWD (Singalong here!);
RETURN

[VERSE1]
Command line wizards, we’re starting it right,
[...]
Flag on the horizon, what challenge is next?

REFRAIN;

Echoes in memory, packets in trace,
[...]
Buried deep in the noise, but we will prevail.

REFRAIN;

Binary sorcerers, let’s tear it apart,
[...]
Patch it up right, and watch the lock release.

REFRAIN;

Ciphertext tumbling, breaking the spin,
[...]
Decrypt that flag and hear the ether call.

REFRAIN;

SQL injection, XSS flow,
[...]
In the world wide labyrinth, we’re never lost.

REFRAIN;

Stack's overflowing, breaking the chain,
[...]
Control the instruction, flags call my name.

REFRAIN;

END;
'''

MAX_LINES = 100

def reader(song, startLabel):
  lip = 0
  start = 0
  refrain = 0
  refrain_return = 0
  finished = False

  # Get list of lyric lines
  song_lines = song.splitlines()
  
  # Find startLabel, refrain and refrain return
  for i in range(0, len(song_lines)):
    if song_lines[i] == startLabel:
      start = i + 1
    elif song_lines[i] == '[REFRAIN]':
      refrain = i + 1
    elif song_lines[i] == 'RETURN':
      refrain_return = i

  # Print lyrics
  line_count = 0
  lip = start
  while not finished and line_count < MAX_LINES:
    line_count += 1
    for line in song_lines[lip].split(';'):
      if line == '' and song_lines[lip] != '':
        continue
      if line == 'REFRAIN':
        song_lines[refrain_return] = 'RETURN ' + str(lip + 1)
        lip = refrain
      elif re.match(r"CROWD.*", line):
        crowd = input('Crowd: ')
        song_lines[lip] = 'Crowd: ' + crowd
        lip += 1
      elif re.match(r"RETURN [0-9]+", line):
        lip = int(line.split()[1])
      elif line == 'END':
        finished = True
      else:
        print(line, flush=True)
        time.sleep(0.5)
        lip += 1



reader(song_flag_hunters, '[VERSE1]')
```

It seems to be a song reading line by line. They limit the number of lines to be sang with:

```python
MAX_LINES = 100
```

So after 100 lines the program halts, to be sure not falling into an infinite loop inside the `while`.

Inside it, it splits the `if` checks with the char ";" (semicolon), the possible matches are:

- 'REFRAIN': Return to the start of a section of the lyrics
- 'CROWD': Check for input of the user.
	- Usually we can find a vulnerability in Python 2, where the input can read a variable, but this is not the case, we are dealing with python 3.
- 'RETURN \[number\]': Return to a specific line
- 'END': Stop the singing/lyrics/lines. Also stops the program.

And in the start of the program, we have:

```python
# Read in flag from file
flag = open('flag.txt', 'r').read()

secret_intro = \
'''Pico warriors rising, puzzles laid bare,
Solving each challenge with precision and flair.
With unity and skill, flags we deliver,
The ether’s ours to conquer, '''\
+ flag + '\n'

song_flag_hunters = secret_intro +\
```

Where:
- `flag = open('flag.txt', 'r').read()`: Reads the flag.txt file, where our goal resides
- `secret_intro`: Is a lyrics section where the flag is stored, but not acessible
- `song_flag_hunters`: Is the lyrics the user sees, but with the `secret_intro` appended to the string.

So our string have the flag, but not accessible to the user.

If we insert the input at "CROWD" section `;END;`, the program will interpret the ";" as a new line, and also will reproduce the "END" string as a command.

This will result in the program being halted, ending the execution.

So, maybe we can use that as well with the command `RETURN [0-9]`, to return to a specific line in the lyrics. Let's return to the line '1', I will use echo to append the string to the `nc` connection:

```bash
echo ";RETURN 1;" | nc verbal-sleep.picoctf.net 69420
```

With that, the lyrics return to our `secret_intro` part, revealing the flag.


{{% details title="Answer: Flag" open=false %}}
```
picoCTF{70637h3r_f0r3v3r_befbccb7}
```
{{% /details %}}
