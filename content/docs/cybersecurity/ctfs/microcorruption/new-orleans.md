---
title: "New Orleans"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Microcorruption - New Orleans

Microcorruption gives us a debugger and a password to unlock a device, our job is to find the password reading through assembly code and using some reverse engineering tricks. Each challenge is named after a city, each one more difficult than the previous one.

If you didn't solve the challenge for yourself, I recommend that you stop reading, solve the challenge and check my solution afterwards, do not spoil the challenge and have fun.

## How to solve it

Always start from the main function, the `main` is our starting point, we can see that:

```nasm
main:
	add		#0xff9c, sp
	call	#0x447e <create_password>
	mov		#0x44e4 "Enter the password to continue", r15
	call	#0x4594 <puts>
	mov		sp, r15
	call	#0x44b2 <get_password>
	mov		sp, r15
	call	#0x44bc <check_password>
	tst		r15
	jnz		$+0xc <main+0x2a>
	mov		#0x4503 "Invalid password; try again.", r15
	call	#0x4594 <puts>
	jmp		$+0xe <main+0x36>
	mov		#0x4520 "Access Granted!", r15
	call	#0x4594 <puts>
	call	#0x44d6 <unlock_door>
	clr		r15
	add		#0x64, sp
```

Reading through the instructions, we can see the following commands:

- Creates a password
- Ask for a password
- Compares the password

What calls my attention is that the program is creating a password at runtime, this means that is being stored in the memory? First let's check the function `create_password`.

```nasm
create_password:
	mov		#0x2400, r15
	mov.b	#0x2d, 0x0(r15)
	mov.b	#0x41, 0x1(r15)
	mov.b	#0x44, 0x2(r15)
	mov.b	#0x3e, 0x3(r15)
	mov.b	#0x5d, 0x4(r15)
	mov.b	#0x6b, 0x5(r15)
	mov.b	#0x63, 0x6(r15)
	mov.b	#0x0, 0x7(r15)
	ret
```

The function `create_password` is really writing the password in the memory, line by line in hexadecimal until reach the value `0x0`, indicating the null byte, as the end of the password. 

If we make an annotation of the following bytes and enable the Hex Encoded Input, we can input our password and finish this level.

Remember to not add the 0x0, this is not part of the password, just an indicator.

{{% details title="Answer:New Orleans" open=false %}}
```
2d41443e5d6b63
```
{{% /details %}}
