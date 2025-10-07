---
title: "Low Level Binary Intro: Warmup"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# PicoCTF: Low level Binary Intro - Warmup

I'm coming back to the [PicoCTF Playlist](https://play.picoctf.org/playlists/2) challenges, and finishing the Warmup list. So in this text we will go through the challenges and solving them.

---

## ASCII Numbers

For this challenge, I'll need to get a collection of Hexadecimal values and convert it to ASCII characters and reveal the flag. I could convert by hand, but I want to do a python script for that.

First, we need to Trim the Hexadecimal values like "0x30 0x40" to be a list array of only simple hex values like [30,40]. That's the code for that:

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

{% spoiler Answer:ASCII Numbers %}
picoCTF{45c11_n0_qu35710n5_1ll_t311_y3_n0_l135_445d4180}
{% endspoiler %}

---

## Picker I

This is a random number generator service, this one have a NetCat call from the PicoCTF servers, with that it also provide us a source code (python script). 

Accessing the netcat, provides me a possibility to roll a getRandomNumber(), but always return 4. It's really random? [We will never know.](https://xkcd.com/221/)

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/dl4he12s0ycqhaiivmde.png)

Looking through the source file, we can check that the getRandomNumber() is useless, but it also has a method called "win()" which call a file called "flag.txt". Wait, we want that.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/tii8sh56oj3vwrcjp3v9.png)

calling this method instead getRandomNumber, we receive a bunch of hex values:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fb1d1ch6d98gltgmhn8y.png)

Lucky us, we have a script specifically for that, so calling the command:

```bash
python3 HexToASCII.py "0x70 0x69 0x63 0x6f 0x43 0x54 0x46 0x7b 0x34 0x5f 0x64 0x31 0x34 0x6d 0x30 0x6e 0x64 0x5f 0x31 0x6e 0x5f 0x37 0x68 0x33 0x5f 0x72 0x30 0x75 0x67 0x68 0x5f 0x36 0x65 0x30 0x34 0x34 0x34 0x30 0x64 0x7d"
```
We have a result, and the response is our flag:

{% spoiler Answer:Picker I %}
picoCTF{4_d14m0nd_1n_7h3_r0ugh_6e04440d}
{% endspoiler %}
