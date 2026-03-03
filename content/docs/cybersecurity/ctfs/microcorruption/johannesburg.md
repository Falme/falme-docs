---
title: "Johannesburg"
weight: 8
# bookFlatSection: false
# bookToc: true
# bookHidden: true
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Microcorruption - Johannesburg

Microcorruption gives us a debugger and a password to unlock a device, our job is to find the password reading through assembly code and using some reverse engineering tricks. Each challenge is named after a city, each one more difficult than the previous one.

If you didn't solve the challenge for yourself, I recommend that you stop reading, solve the challenge and check my solution afterwards, do not spoil the challenge and have fun.

## How to solve it

As always, first we are going to check the `main` function:

```
4438 <main>
	call	#0x452c <login>
```

The only thing it does is to call the `login` function, so let's check that too:

```
452c <login>
	add		#0xffee, sp
	mov.b	#0xaa, 0x11(sp)
	mov		#0x447c "Enter the password to continue.", r15
	call	#0x45f8 <puts>
	mov		#0x449c "Remember: passwords are between 8 and 16 characters.", r15
	call	#0x45f8 <puts>
	mov		#0x3f, r14
	mov		#0x2400, r15
	call	#0x45e8 <getsn>
	mov		#0x2400, r14
	mov		sp, r15
	call	#0x4624 <strcpy>
	mov		sp, r15
	call	#0x4452 <test_password_valid>
	tst		r15
	jz		$+0xc <login+0x44>
	call	#0x4446 <unlock_door>
	mov		#0x44d1 "Access granted.", r15
	jmp		$+0x6 <login+0x48>
	mov		#0x44e1 "That password is not correct.", r15
	call	#0x45f8 <puts>
	cmp.b	#0xaa, 0x11(sp)
	jz		$+0xe <login+0x60>
	mov		#0x44ff "Invalid Password Length: password too long.", r15
	call	#0x45f8 <puts>
	br		#0x443c <__stop_progExec__>
	add		#0x12, sp
	ret
```

So, what it does, in order, is:

- Print text asking for the password with `<puts>`
- Prompt asking for the password input
- copy a string to another place `<strcpy>` (meaning "string copy")
- Test if the password is valid `<test_password_valid>`
- If the password is correct, open the door
- If the password is not correct, stop the program

Knowing that, let's see which part of the program we can reach using a buffer overflow. Let's write the number `6` a bunch of times in the password input:

```
6666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666
```

After the input and making a `step` command, we can see that our input is being written in the memory between the addresses `2400` and `243e`.

Now, we will pass through the function `<strcpy>`, to see what it does:

```
4624 <strcpy>
	mov		r15, r13
	jmp		$+0x6 <strcpy+0x8>
	inc		r14
	inc		r13
	mov.b	@r14, r12
	mov.b	r12, 0x0(r13)
	tst.b	r12
	jnz		$-0xc <strcpy+0x4>
	ret
```

After some testing, we can see that it copies our password input to another address in the memory. The memory is being written between the addresses `43ec` and `442a`.

The problem resides in the `<test_password_valid>`. It never checks the values we inserted as password. We need to find another place.

Let's check the compares (`cmp`) again, maybe we can find an useful way to reach our solution.

At `login` function we can find a compare that is checked even if our password is incorrect:

```
	cmp.b	#0xaa, 0x11(sp)
	jz		$+0xe <login+0x60>
	mov		#0x44ff "Invalid Password Length: password too long.", r15
	call	#0x45f8 <puts>
	br		#0x443c <__stop_progExec__>
	add		#0x12, sp
	ret
```

This checks if the password is too long, but it also makes a literal comparison with the byte `0xaa`, so let's add a bunch of `0xaa` in the password to see what it does:


```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

This time, the code will not reach the `<__stop_progExec__>` and halt the program, actually it will go to the `add #0x12, sp` and return. If we check the `sp` position in the memory after this addition, we can see that it does appear between the addresses `43ec` and `442a`, the addresses where the `<strcpy>` function copied our password.

As you should know, the `ret` operation takes a value from the `sp` (stack pointer) to know where to return. Maybe if we inserted the address of the function to unlock the door at this `sp` position, we can solve this one. So, this is the unlock door function:

```
4446 <unlock_door>
	push	#0x7f
	call	#0x4594 <INT>
	incd	sp
	ret
```

The function to open the door is at `0x4446`, because of endianness, we are going to insert as `0x4644`. The input should be something like this:

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa4644
```

Inserting it to the password, the door should open.
