---
title: "Cusco"
weight: 4
# bookFlatSection: false
# bookToc: true
# bookHidden: true
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Microcorruption - Cusco

Microcorruption gives us a debugger and a password to unlock a device, our job is to find the password reading through assembly code and using some reverse engineering tricks. Each challenge is named after a city, each one more difficult than the previous one.

If you didn't solve the challenge for yourself, I recommend that you stop reading, solve the challenge and check my solution afterwards, do not spoil the challenge and have fun.

## How to solve it

First of all, the `main` function just redirects to the `login` function, so let's check what it does:

```nasm
login:
	add		#0xfff0, sp
	mov		#0x447c "Enter the password to continue.", r15
	call	#0x45a6 <puts>
	mov		#0x449c "Remember: passwords are between 8 and 16 characters.", r15
	call	#0x45a6 <puts>
	mov		#0x30, r14
	mov		sp, r15
	call	#0x4596 <getsn>
	mov		sp, r15
	call	#0x4452 <test_password_valid>
	tst		r15
	jz		$+0xc <login+0x32>
	call	#0x4446 <unlock_door>
	mov		#0x44d1 "Access granted.", r15
	jmp		$+0x6 <login+0x36>
	mov		#0x44e1 "That password is not correct.", r15
	call	#0x45a6 <puts>
	add		#0x10, sp
	ret
```

the `login` function does the following:
- print a text asking for a password (8 to 16 characters)
- asks for a password
- check if the password is valid
- print the result (if was a success or not)
- finish the function with a return

The console asks us for a password between 8 and 16 characters. Let's check if 16 characters is the limit for our input, I'm gonna write a bunch of sevens and then check the memory:

```
43e0: 5645 0000 a045 0200 ee43 3000 1e45 3737   VE...E...C0..E77
43f0: 3737 3737 3737 3737 3737 3737 3737 3737   7777777777777777
4400: 3737 3737 3737 3737 3737 3737 3737 3737   7777777777777777
4410: 3737 3737 3737 3737 3737 3737 3737 0045   77777777777777.E
```

Well, we can actually write 48 characters in this input, way above the expected, reaching from the address `0x43ee` to the address `0x441d`.

The problem is, in any way shape or form, the function `test_password_valid` make an interaction with our input interval. Even if you track the r15 to see what it does, the compare seems to never happen:

```nasm
call	#0x4452 <test_password_valid>
tst		r15
jz		$+0xc <login+0x32>
call	#0x4446 <unlock_door>
mov		#0x44d1 "Access granted.", r15
```

In another words, the register `r15` is always zero, making it skip the `unlock_door` function, the one we desire.

But, after that, the program seems to halt. Ok, let's check that again.

What we can see is that the RET (return) operation is calling the return at the address `0x43fe`, that means 2 things.

1. If we input more than 16 characters into the password, this data is corrupted by our sevens
2. The return position call is between `0x43ee` and `0x441d`, making possible for us to manipulate this return.

So, let's try to change the return to send us to `unlock_door` function, let's find the address for the `unlock_door` function first:

```nasm
4446 <unlock_door>
4446:  3012 7f00      push	#0x7f
444a:  b012 4245      call	#0x4542 <INT>
444e:  2153           incd	sp
4450:  3041           ret
```

So the address is `0x4446`, let's inject this address to the position 17 of bytes (16 bytes of zeros + 1 byte of `unlock_door` address).

Remember to invert the bytes, the address is `0x4446`, but our input will be `0x4644`:

```
000000000000000000000000000000004644
```

With that, the return operation from the `login` function will return to the `unlock_door` function, opening the door.
