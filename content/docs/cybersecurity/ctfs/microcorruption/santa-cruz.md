---
title: "Santa Cruz"
weight: 9
# bookFlatSection: false
# bookToc: true
# bookHidden: true
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Microcorruption - Santa Cruz

Microcorruption gives us a debugger and a password to unlock a device, our job is to find the password reading through assembly code and using some reverse engineering tricks. Each challenge is named after a city, each one more difficult than the previous one.

If you didn't solve the challenge for yourself, I recommend that you stop reading, solve the challenge and check my solution afterwards, do not spoil the challenge and have fun.

## How to solve it

As always, let's check the `<main>` function:

```
4438 <main>
4438:  3150 ceff      add	#0xffce, sp
443c:  b012 5045      call	#0x4550 <login>
```

The `<main>` function just calls the `<login>` function. Let's check that one too:

```
4550 <login>
4550:  0b12           push	r11
4552:  0412           push	r4
4554:  0441           mov	sp, r4
4556:  2452           add	#0x4, r4
4558:  3150 d8ff      add	#0xffd8, sp
455c:  c443 faff      mov.b	#0x0, -0x6(r4)
4560:  f442 e7ff      mov.b	#0x8, -0x19(r4)
4564:  f440 1000 e8ff mov.b	#0x10, -0x18(r4)
456a:  3f40 8444      mov	#0x4484 "Authentication now requires a username and password.", r15
456e:  b012 2847      call	#0x4728 <puts>
4572:  3f40 b944      mov	#0x44b9 "Remember: both are between 8 and 16 characters.", r15
4576:  b012 2847      call	#0x4728 <puts>
457a:  3f40 e944      mov	#0x44e9 "Please enter your username:", r15
457e:  b012 2847      call	#0x4728 <puts>
4582:  3e40 6300      mov	#0x63, r14
4586:  3f40 0424      mov	#0x2404, r15
458a:  b012 1847      call	#0x4718 <getsn>
458e:  3f40 0424      mov	#0x2404, r15
4592:  b012 2847      call	#0x4728 <puts>
4596:  3e40 0424      mov	#0x2404, r14
459a:  0f44           mov	r4, r15
459c:  3f50 d6ff      add	#0xffd6, r15
45a0:  b012 5447      call	#0x4754 <strcpy>
45a4:  3f40 0545      mov	#0x4505 "Please enter your password:", r15
45a8:  b012 2847      call	#0x4728 <puts>
45ac:  3e40 6300      mov	#0x63, r14
45b0:  3f40 0424      mov	#0x2404, r15
45b4:  b012 1847      call	#0x4718 <getsn>
45b8:  3f40 0424      mov	#0x2404, r15
45bc:  b012 2847      call	#0x4728 <puts>
45c0:  0b44           mov	r4, r11
45c2:  3b50 e9ff      add	#0xffe9, r11
45c6:  3e40 0424      mov	#0x2404, r14
45ca:  0f4b           mov	r11, r15
45cc:  b012 5447      call	#0x4754 <strcpy>
45d0:  0f4b           mov	r11, r15
45d2:  0e44           mov	r4, r14
45d4:  3e50 e8ff      add	#0xffe8, r14
45d8:  1e53           inc	r14
45da:  ce93 0000      tst.b	0x0(r14)
45de:  fc23           jnz	$-0x6 <login+0x88>
45e0:  0b4e           mov	r14, r11
45e2:  0b8f           sub	r15, r11
45e4:  5f44 e8ff      mov.b	-0x18(r4), r15
45e8:  8f11           sxt	r15
45ea:  0b9f           cmp	r15, r11
45ec:  0628           jnc	$+0xe <login+0xaa>
45ee:  1f42 0024      mov	&0x2400, r15
45f2:  b012 2847      call	#0x4728 <puts>
45f6:  3040 4044      br	#0x4440 <__stop_progExec__>
45fa:  5f44 e7ff      mov.b	-0x19(r4), r15
45fe:  8f11           sxt	r15
4600:  0b9f           cmp	r15, r11
4602:  062c           jc	$+0xe <login+0xc0>
4604:  1f42 0224      mov	&0x2402, r15
4608:  b012 2847      call	#0x4728 <puts>
460c:  3040 4044      br	#0x4440 <__stop_progExec__>
4610:  c443 d4ff      mov.b	#0x0, -0x2c(r4)
4614:  3f40 d4ff      mov	#0xffd4, r15
4618:  0f54           add	r4, r15
461a:  0f12           push	r15
461c:  0f44           mov	r4, r15
461e:  3f50 e9ff      add	#0xffe9, r15
4622:  0f12           push	r15
4624:  3f50 edff      add	#0xffed, r15
4628:  0f12           push	r15
462a:  3012 7d00      push	#0x7d
462e:  b012 c446      call	#0x46c4 <INT>
4632:  3152           add	#0x8, sp
4634:  c493 d4ff      tst.b	-0x2c(r4)
4638:  0524           jz	$+0xc <login+0xf4>
463a:  b012 4a44      call	#0x444a <unlock_door>
463e:  3f40 2145      mov	#0x4521 "Access granted.", r15
4642:  023c           jmp	$+0x6 <login+0xf8>
4644:  3f40 3145      mov	#0x4531 "That password is not correct.", r15
4648:  b012 2847      call	#0x4728 <puts>
464c:  c493 faff      tst.b	-0x6(r4)
4650:  0624           jz	$+0xe <login+0x10e>
4652:  1f42 0024      mov	&0x2400, r15
4656:  b012 2847      call	#0x4728 <puts>
465a:  3040 4044      br	#0x4440 <__stop_progExec__>
465e:  3150 2800      add	#0x28, sp
4662:  3441           pop	r4
4664:  3b41           pop	r11
4666:  3041           ret
```

Damn, this is a big function. Let's check what it does:

- Print on Screen some texts asking for username with `<puts>`
- Set position in memory to be written at `0x2404`
- Ask the user with a prompt for a username using `<getsn>`
- Print on screen the username typed
- Set a new position in memory to copy the username using `<strcpy>`
- Set position in memory to be written at `0x2404`
- Ask the user with a prompt for a password using `<getsn>`
- Print on screen the password typed
- Set a new position in memory to copy the password using `<strcpy>`
- Read all the value from the input until reached a null byte (`0x00`)
- If it's too long, stop the program and print that "the password is too long"
- If not, check if the password is correct, if it's not, stop the program and print "That password is not correct"
- If the password is correct, call the `<unlock_door>` function, opening the door and solving the problem.

Knowing that, let's check how long our information is being registered in memory adding a bunch of '6's in the username and password.

After some testing we can see that:

- The username:
	- Before the copy, can be written between the addresses `0x2404` and `0x2466`
	- After the copy, can be written between the addresses `0x43a2` and `0x4404`
- The password:
	- Before the copy, can be written between the addresses `0x2404` and `0x2466`
	- After the copy, can be written between the addresses `0x43b5` and `0x4417`

The distance is about `0x63` in size, probably setted before each call for `<getsn>` on `r14` register:

```
4582:  3e40 6300      mov	#0x63, r14
```

### How strcpy Works

After we insert our input, the values is copied to another address. Here's the function:

```
4754 <strcpy>
4754:  0d4f           mov	r15, r13
4756:  023c           jmp	$+0x6 <strcpy+0x8>
4758:  1e53           inc	r14
475a:  1d53           inc	r13
475c:  6c4e           mov.b	@r14, r12
475e:  cd4c 0000      mov.b	r12, 0x0(r13)
4762:  4c93           tst.b	r12
4764:  f923           jnz	$-0xc <strcpy+0x4>
4766:  3041           ret
```

Pretty simple, it just get one address position, move one byte of value from one address to another until the copied byte is not zero, and if the byte is zero, then return.

This means that, if we input some value like:

```
6666009999
```

The values `9999` from that input will not be copied, because there's a null/zero byte (`0x00`), and the copy function `<strcpy>` return after finding the zero byte.

This causes a limitation on our attack, we must be cautious when or if we use `00` in our input.


### How the door opens

Unlikely some other challenges, this one really opens at `<login>` call for `<unlock_door>`.

```
463a:  b012 4a44      call	#0x444a <unlock_door>
```

So, our objective is to reach that line or make a possible call directly to `<unlock_door>`.


### How it limits the input size (8 to 16 characters)

After checking the area where's the username and password is copied, we can find two lonely bytes at `0x43b3` and `0x43b4`, with the values `0x08` and `0x10` respectively.

What a coincidence that `0x08` is 8 in decimal, and `0x10` is 16 in decimal. If we need to input a longer username and password, we must change this value. Imagine we change the first value to `0x01` and the second value to `0x30`, this gives us more possibility to play with the memory.


### The Checks and Compares to reach our goal

What we know is that, to reach our goal, we must:

- Pass the "Password is too long" check.
- Pass the "That password is not correct" check.
- OR at the end of the `<login>` function, make it return to the `<unlock_door>` function.

The "Password is too long" check happens at `0x45ea` and the "Password is too short" check happens at `0x4600`.

```
45ea:  0b9f           cmp	r15, r11
45ec:  0628           jnc	$+0xe <login+0xaa>
45ee:  1f42 0024      mov	&0x2400, r15
45f2:  b012 2847      call	#0x4728 <puts>
45f6:  3040 4044      br	#0x4440 <__stop_progExec__>
45fa:  5f44 e7ff      mov.b	-0x19(r4), r15
45fe:  8f11           sxt	r15

4600:  0b9f           cmp	r15, r11
4602:  062c           jc	$+0xe <login+0xc0>
4604:  1f42 0224      mov	&0x2402, r15
4608:  b012 2847      call	#0x4728 <puts>
460c:  3040 4044      br	#0x4440 <__stop_progExec__>
```

If we can modify the values of the Minimum and Maximum characters input, this will no longer be a problem, So, for the username, we can write an input like : 

```
66666666666666666666666666666666660130
```

Making the Minimum as `0x01` and the Maximum as `0x30`.

Now, for the "That password is not correct" check, I must say upfront, we are not going to make this true, we are going to fail this check so we can make a return directly to the `<unlock_door>` function.

The return position of the function `<login>` is at `0x43c0`, a position that we can insert our input values. We just need to modify these values.

The address for the `<unlock_door>` function is `0x444a`, we invert the address because of endianness, so the input will be looking something like:

```
6666666666666666666666666666666666013066666666666666666666666666666666666666666666664a44
```

One last thing, The last check at the address `0x4650` check if an address is zero `0x00`, if not, it just halt the program and does not reach the return. We must have zero at the memory address `0x4650`. But we cannot add a zero in the middle of the input, right?

That's why this problem have two inputs, username and password, we are going to make them work together. The username will override the most of values, and the password will just add the necessary zero to `0x4650`. We are going to have the username and password to be:

```
user:
6666666666666666666666666666666666013066666666666666666666666666666666666666666666664a44

pass:
6666666666666666666666666666666666
```

But there's no zero in the password? Yes, the zero byte will be added automatically, we just need to know where to stop our input.

After all that, we input the username and password, and the door opens. Congratulations!
