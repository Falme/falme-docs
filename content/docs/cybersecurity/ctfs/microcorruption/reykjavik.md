---
title: "Reykjavik"
weight: 5
# bookFlatSection: false
# bookToc: true
# bookHidden: true
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Microcorruption - Reykjavik

Microcorruption gives us a debugger and a password to unlock a device, our job is to find the password reading through assembly code and using some reverse engineering tricks. Each challenge is named after a city, each one more difficult than the previous one.

If you didn't solve the challenge for yourself, I recommend that you stop reading, solve the challenge and check my solution afterwards, do not spoil the challenge and have fun.

## How to solve it

First of all, let's check the `main` function:

```
4438 <main>
4438:  3e40 2045      mov	#0x4520, r14
443c:  0f4e           mov	r14, r15
443e:  3e40 f800      mov	#0xf8, r14
4442:  3f40 0024      mov	#0x2400, r15
4446:  b012 8644      call	#0x4486 <enc>
444a:  b012 0024      call	#0x2400
444e:  0f43           clr	r15
```

After moving some data, the `main` function makes 2 calls: `0x4486 <enc>` and `#0x2400`.

The funny part is that, the `enc` function is on the disassembly list, but the call at `0x2400` is not available before running the script. 

If we check out the `enc` function, we can see that it's moving some bytes around, first listing all the possible byte and then reordering at the memory. We can make the assumption that the `enc` function means "encryption", and it's reordering the bytes to form a new code, a code that will be called by the `main` function afterwards.

Just after calling the `enc` function, the call for the address `0x2400` will be written in the memory and possible to be called.

So add a breakpoint on the `#0x2400` call:

```
444a:  b012 0024      call	#0x2400 <-- here
```

Then, we can see that the `enc` function has written to the memory through the addresses `0x2400` to `0x2570`. If we copy the memory in this range and go to the page [(DIS)ASSEMBLER](https://microcorruption.com/assembler), we can input the memory values into the disassembler to retrieve an assembly code, which we can read and understand what it does.

### Disassembled Memory Block

The disassembler will show many lines of code, this will look intimidating, but we can analyze step-by-step, we can still use the "step" action in the program, and look at "Current Instruction" (top-right of the page), showing the current instruction in assembly.

If we step through the program code looking for the `cmp` command, we can find only two. This means that only 2 places is comparing something to check if the password is correct or wrong. The two places are:

```
b490 45ea dcff cmp	#0xea45, -0x24(r4)
fa9b 6674      cmp.b	@r11+, 0x7466(r10)
```

The compares are:
- #0xea45, -0x24(r4)
	- Literal value `0xea45` with address `-0x24(r4)`
- @r11+, 0x7466(r10)
	- value `@r11+` with address `0x7466(r10)`

First, let's try inserting the literal value of the first compare. So we are going to input the values `0x45ea` (inverted `0xea45` little endian). 

```
45ea45ea45ea45ea45ea45ea45ea45ea45ea45ea45ea
```

After inputting it, the locked door opens, we found our solution.

But, maybe we can make it shorter? Let's try an only `45ea`:

```
45ea
```

That also opens the locked door, this is cleaner and pretty.
