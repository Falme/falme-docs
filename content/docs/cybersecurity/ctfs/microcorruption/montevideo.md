---
title: "Montevideo"
weight: 7
# bookFlatSection: false
# bookToc: true
# bookHidden: true
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Microcorruption - Montevideo

Microcorruption gives us a debugger and a password to unlock a device, our job is to find the password reading through assembly code and using some reverse engineering tricks. Each challenge is named after a city, each one more difficult than the previous one.

If you didn't solve the challenge for yourself, I recommend that you stop reading, solve the challenge and check my solution afterwards, do not spoil the challenge and have fun.

## How to solve it

The solution for this problem is *almost* identical to the [Whitehorse](../whitehorse/), so check that one before check this one.

So, the answer is the same, but the catch is one difference. The input that we insert is being copied from one place of the memory to another place in memory.

Our input is being registered between `0x2400` and `0x242f`.
The copy of our input is being copied between `0x43ee` and `0x441d`.

The problem is, the function that copies our input (`<strcpy>`) halts if a zero-byte is found. So, in the input from Whitehorse, the value was:

```
666666666666666666666666666666664c450000ff00
```

But we cannot use the zeroes here, or the value `ff00` will not be copied. Let's replace the zeroes with more 6's.

```
666666666666666666666666666666664c456666ff00
```

With that, the door will be unlocked.

