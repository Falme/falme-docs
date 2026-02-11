---
title: "Hanoi"
weight: 3
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Microcorruption - Hanoi

Microcorruption gives us a debugger and a password to unlock a device, our job is to find the password reading through assembly code and using some reverse engineering tricks. Each challenge is named after a city, each one more difficult than the previous one.

If you didn't solve the challenge for yourself, I recommend that you stop reading, solve the challenge and check my solution afterwards, do not spoil the challenge and have fun.

## How to solve it

First of all, let's check the `main` function:

```nasm
main:
	call	#0x4520 <login>
	clr		r15
```

The `main` function just redirect to another function called `login`, let's check that one too:

```nasm
login:
	mov.b	#0x0, &0x2410
	mov		#0x447e "Enter the password to continue.", r15
	call	#0x45de <puts>
	mov		#0x449e "Remember: passwords are between 8 and 16 characters.", r15
	call	#0x45de <puts>
	mov		#0x1c, r14
	mov		#0x2400, r15
	call	#0x45ce <getsn>
	mov		#0x2400, r15
	call	#0x4454 <test_password_valid>
	tst		r15
	jz		$+0x8 <login+0x32>
	mov.b	#0x2b, &0x2410
	mov		#0x44d3 "Testing if password is valid.", r15
	call	#0x45de <puts>
	cmp.b	#0x5d, &0x2410
	jnz		$+0x10 <login+0x50>
	mov		#0x44f1 "Access granted.", r15
	call	#0x45de <puts>
	call	#0x4448 <unlock_door>
	ret
	mov		#0x4501 "That password is not correct.", r15
	call	#0x45de <puts>
	ret
```


There's the sequence we are looking for, in simple terms, this code:
- write a text that you need to enter a password (8 to 16 characters)
- asks for a password
- check if the password is valid

Well, the program is asking for us to input a password between 8 and 16 characters, maybe that's some kind of tip for us?

Running through the code trying different types and sizes of password, we can see that our password is being stored at the address `0x2400`. Besides that, we are actually not limited to only 16 characters. The memory can actually store 28 characters, way above 16 characters.

We can also see that the decisive compare in the `login` function is comparing a byte at `0x2410`:

```nasm
cmp.b	#0x5d, &0x2410
jnz		$+0x10 <login+0x50>
mov		#0x44f1 "Access granted.", r15
```

The address `0x2410` is being compared to the byte `0x5d`, if the comparison is correct (if it's equal, zero) we reach the "Access granted." and unlock the Lock. The address `0x2410` is very close to the address where our password is being stored (`0x2400`). Maybe we can buffer overflow to that address?

From the address `0x2400` to `0x2410` is 16 bytes, so we are going to do 16 bytes of 0x0's + 1 bytes of 0x5d, like this:

```
000000000000000000000000000000005d
```

With that we just skip the comparison, and the real password does not matter, we unlocked this lock.
