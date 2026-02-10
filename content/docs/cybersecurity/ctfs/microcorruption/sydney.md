---
title: "Sydney"
weight: 2
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Microcorruption - Sydney

Microcorruption gives us a debugger and a password to unlock a device, our job is to find the password reading through assembly code and using some reverse engineering tricks. Each challenge is named after a city, each one more difficult than the previous one.

If you didn't solve the challenge for yourself, I recommend that you stop reading, solve the challenge and check my solution afterwards, do not spoil the challenge and have fun.

## How to solve it

First of all, let's check what the `main` function is doing:

```nasm
main:
	add		#0xff9c, sp
	mov		#0x44b4 "Enter the password to continue.", r15
	call	#0x4566 <puts>
	mov		sp, r15
	call	#0x4480 <get_password>
	mov		sp, r15
	call	#0x448a <check_password>
	tst		r15
	jnz		$+0xc <main+0x26>
	mov		#0x44d4 "Invalid password; try again.", r15
	call	#0x4566 <puts>
	jmp		$+0x14 <main+0x38>
	mov		#0x44f1 "Access Granted!", r15
	call	#0x4566 <puts>
	push	#0x7f
	call	#0x4502 <INT>
	incd	sp
	clr		r15
	add		#0x64, sp
```

In a few steps, we can see that:
- Write text asking for password
- asks for a password
- compare the password

The function that prints on screen and the `get_password` have nothing uncommon, so let's check the `check_password` function in more details:

```nasm
check_password:
	cmp		#0x5b7b, 0x0(r15)
	jnz		$+0x1c <check_password+0x22>
	cmp		#0x7477, 0x2(r15)
	jnz		$+0x14 <check_password+0x22>
	cmp		#0x703d, 0x4(r15)
	jnz		$+0xc <check_password+0x22>
	mov		#0x1, r14
	cmp		#0x2355, 0x6(r15)
	jz		$+0x4 <check_password+0x24>
	clr		r14
	mov		r14, r15
	ret
```

It looks like it compares a literal byte (#) with the r15 register memory data. So, it does something like:
- Compares if the bytes at (r15 + 0 bytes) equals 0x5b7b
- Compares if the bytes at (r15 + 2 bytes) equals 0x7477
- Compares if the bytes at (r15 + 4 bytes) equals 0x703d
- Compares if the bytes at (r15 + 6 bytes) equals 0x2355

The r15 position in the memory is where our password is stored. So, in theory, if we input these bytes in order (`5b7b7477703d2355`), the lock will open, right? Well, there's a problem.

We are reading byte-by-byte from the memory address, which means the bytes must be inverted:
- 0x5b7b becomes 0x7b5b
- 0x7477 becomes 0x7774
- 0x703d becomes 0x3d70
- 0x2355 becomes 0x5523

If we input in the correct arrangement, we have the final answer for our challenge.

{{% details title="Answer:Sydney" open=false %}}
```
7b5b77743d705523
```
{{% /details %}}
