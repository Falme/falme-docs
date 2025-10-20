---
title: "Warmup"
weight: 3
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# PicoCTF: Low Level Binary Intro - Warmup

I'm starting to tackle the challenges of PicoCTF, a website to challenge myself on Capture the Flag puzzles (CTFs), learning the basics of cybersecurity and maybe make it useful for game development, web development and life in general.

So the first ones are very basic, the idea is to make a step-by-step learning routine, and for that, I'll be starting at the Playlists page.

![Image description](/images/low-level-binary-intro-warmup/1.jpg)

As it says in the webpage: "Playlists are collections of challenges, sometimes with readings or games, that are curated to help students learn a particular topic."

So I'll start with <a href='https://play.picoctf.org/playlists/2' target='_blank'>"Low Level Binary Intro"</a>, that can be helpful with my current job (Game Development).

The first section of the Low Level Binary Intro is called "Warmup", we will be covering it in this document.

## Flag 1: Obedient Cat

![Image description](/images/low-level-binary-intro-warmup/2.jpg)

This one is just a sanity check for the other challenges.

Sanity Check is a simple test to check if everything is happening as expected on the basic level. In this case, I just need to download the flag in a file and read the content inside. There's a flag for this one.

{{% details title="Answer:Flag 1" open=false %}}
```
picoCTF{s4n1ty_v3r1f13d_b5aeb3dd}
```
{{% /details %}}

---

## Flag 2: Warmed Up

After the sanity check, we reach the first challenge of this post, called "Warmed Up". 

![Image description](/images/low-level-binary-intro-warmup/3.jpg)

This challenge is really easy, just using the DuckDuckGo web search as "0x3D in decimal" should give us the answer. But I know that I'll need to use python in this playlist. So, I'll go further and create a Hexadecimal to Decimal converter in python.

I currently have `Python 3.10.8` version. So let's begin.

The idea is to make a Hex string and each char value multiply by 16.

For example: 0xA9D = A\*16\*16 + 9\*16 + D  
That's the same as = 10\*(16^2) + 9\*(16^1) + 13\*(16^0) = 2717

So first, I'll make a method to convert the single Hex char to a decimal number:

```python
#List/Dictionary of numbers above 9
#To be called from a loop
associationList = [
            ['A', 10],
            ['B', 11],
            ['C', 12],
            ['D', 13],
            ['E', 14],
            ['F', 15],
        ]

def ToDecimal(hexChar):
    
    #First try the string/char range of numbers
    #If there's a match of the key, return value
    for item in associationList:
        if item[0] == hexChar.upper():
            return item[1]

    #Try to return a valid number, if not, return -1
    try:
        return int(hexChar)
    except:
        return 0
```

The Method ToDecimal(char) will make my value 'F' to be converted to 15, but also make my value '6' (as a string) to be converted to 6 (as an integer)

Now I need to create a method that get the Hex value inputted from the user and convert it to decimal values and sum them.

```python
import sys

def HexToDec():
    
    #Total Sum after calculations
    total = 0
 
    #Try to parse the user input, if cannot, show error
    try:
        #Go through all input chars backwards
        for x,char in enumerate(reversed(sys.argv[1])):
            
            #Multiply the respective position Hexadecimal
            #To the decimal value
            multiplier = ToDecimal(char) * pow(16,x)
            
            #Sum/Add to the final result
            total += multiplier

        print(total)
    except:
        print("Not valid arguments or missing argument")

HexToDec()
```

With this code, I can pass the Hexadecimal value through the parameters calling something like : `python HexToDec.py 2e87` and it will return me 11911.

But after making many things that does not solve the original problem, let's go back and solve it:

I need to know the value of 0x3D in Decimal, and putting it in the python code it returns me...

` > 61 `

So I just need to append the 61 to the format of CTF Flag and it's solved:

![Image description](/images/low-level-binary-intro-warmup/4.jpg)

And that's it, a very super complicated solution to a simple problem.

Here's the final [Python File](https://github.com/Falme-SideProjects/python-hex-to-dec/blob/main/HexToDec.py)

{{% details title="Answer:Flag 2" open=false %}}
```
picoCTF{61}
```
{{% /details %}}

---

## Flag 3: ASCII Numbers

For this challenge, I'll need to get a collection of Hexadecimal values and convert it to ASCII characters and reveal the flag. I could convert by hand, but I want to do a python script for that.

First, we need to Trim the Hexadecimal values like "0x30 0x40" to be a list array of only simple hex values like [30,40]. Here's the code for that:

```python

import sys

# Text like "0x30 0x40 0x50" convert to array
def TrimHexString(originalString):

    #Split the values to an array
    hexValues = originalString.split()

    # Check if need to trim the 0x from hex text
    for index,val in enumerate(hexValues):
        # If does have 0x, remove it
        if val[:2] == "0x":
            hexValues[index] = val[2:]

    #return final list array
    return hexValues

TrimHexString("0x10 0x20 0x30")

```

And then taking this list and concatenate to a whole string, like taking from [30,40,50] to "304050". Making possible to call the bytes.fromhex().decode("utf-8") so we can convert the hex to ASCII easily:

```python

# Join all hex arrays to a single string 
def ConcatenateHexList(hexList):
    return "".join(hexList)

```

And then call everything with the first argument from command line and show the results of conversion with decode("utf-8"):

```python

# Get the first value from command line to trim and join
hexConcatenated = ConcatenateHexList(TrimHexString(sys.argv[1]))

# decode the hex to ascii and show results
result = bytes.fromhex(hexConcatenated).decode("utf-8")
print(result)

```

and calling the command:

```bash
python3 HexToASCII.py "0x70 0x69 0x63 0x6f 0x43 0x54 0x46 0x7b 0x34 0x35 0x63 0x31 0x31 0x5f 0x6e 0x30 0x5f 0x71 0x75 0x33 0x35 0x37 0x31 0x30 0x6e 0x35 0x5f 0x31 0x6c 0x6c 0x5f 0x74 0x33 0x31 0x31 0x5f 0x79 0x33 0x5f 0x6e 0x30 0x5f 0x6c 0x31 0x33 0x35 0x5f 0x34 0x34 0x35 0x64 0x34 0x31 0x38 0x30 0x7d"
```

We get the flag:

{{% details title="Answer:Flag 3" open=false %}}
```
picoCTF{45c11_n0_qu35710n5_1ll_t311_y3_n0_l135_445d4180}
```
{{% /details %}}

---

## Flag 4: Picker I

This is a random number generator service, this one have a NetCat call from the PicoCTF servers, with that it also provide us a source code (python script). 

Accessing the netcat, provides me a possibility to roll a getRandomNumber(), but always return 4. Is it really random? [We will never know.](https://xkcd.com/221/)

![Image description](/images/low-level-binary-intro-warmup/5.jpg)

Looking through the source file, we can check that the getRandomNumber() is useless, but it also has a method called "win()" which call a file called "flag.txt". Wait, we want that.

![Image description](/images/low-level-binary-intro-warmup/6.png)

calling this method instead getRandomNumber, we receive a bunch of hex values:

![Image description](/images/low-level-binary-intro-warmup/7.jpg)

Lucky us, we have a script specifically for that, so calling the command:

```bash
python3 HexToASCII.py "0x70 0x69 0x63 0x6f 0x43 0x54 0x46 0x7b 0x34 0x5f 0x64 0x31 0x34 0x6d 0x30 0x6e 0x64 0x5f 0x31 0x6e 0x5f 0x37 0x68 0x33 0x5f 0x72 0x30 0x75 0x67 0x68 0x5f 0x36 0x65 0x30 0x34 0x34 0x34 0x30 0x64 0x7d"
```
We have a result, and the response is our flag:

{{% details title="Answer:Flag 4" open=false %}}
```
picoCTF{4_d14m0nd_1n_7h3_r0ugh_6e04440d}
```
{{% /details %}}
