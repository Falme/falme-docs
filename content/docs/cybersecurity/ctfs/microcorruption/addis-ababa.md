---
title: "Addis Ababa"
weight: 11
# bookFlatSection: false
# bookToc: true
# bookHidden: true
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Microcorruption - Addis Ababa

Microcorruption gives us a debugger and a password to unlock a device, our job is to find the password reading through assembly code and using some reverse engineering tricks. Each challenge is named after a city, each one more difficult than the previous one.

If you didn't solve the challenge for yourself, I recommend that you stop reading, solve the challenge and check my solution afterwards, do not spoil the challenge and have fun.

## How to solve it

First of all, let's check the `main` function:

```
4438 <main>
4438:  3150 eaff      add	#0xffea, sp
443c:  8143 0000      clr	0x0(sp)
4440:  3012 e644      push	#0x44e6
4444:  b012 c845      call	#0x45c8 <printf>
4448:  b140 1b45 0000 mov	#0x451b ">> ", 0x0(sp)
444e:  b012 c845      call	#0x45c8 <printf>
4452:  2153           incd	sp
4454:  3e40 1300      mov	#0x13, r14
4458:  3f40 0024      mov	#0x2400, r15
445c:  b012 8c45      call	#0x458c <getsn>
4460:  0b41           mov	sp, r11
4462:  2b53           incd	r11
4464:  3e40 0024      mov	#0x2400, r14
4468:  0f4b           mov	r11, r15
446a:  b012 de46      call	#0x46de <strcpy>
446e:  3f40 0024      mov	#0x2400, r15
4472:  b012 b044      call	#0x44b0 <test_password_valid>
4476:  814f 0000      mov	r15, 0x0(sp)
447a:  0b12           push	r11
447c:  b012 c845      call	#0x45c8 <printf>
4480:  2153           incd	sp
4482:  3f40 0a00      mov	#0xa, r15
4486:  b012 5045      call	#0x4550 <putchar>
448a:  8193 0000      tst	0x0(sp)
448e:  0324           jz	$+0x8 <main+0x5e>
4490:  b012 da44      call	#0x44da <unlock_door>
4494:  053c           jmp	$+0xc <main+0x68>
4496:  3012 1f45      push	#0x451f
449a:  b012 c845      call	#0x45c8 <printf>
449e:  2153           incd	sp
44a0:  0f43           clr	r15
44a2:  3150 1600      add	#0x16, sp
```

So, let's break down what it does:
- Prints on screen the message asking for username:password with `<printf>`
- Set the limit of characters of input to `0x13` and define input position to be written in memory to be `0x2400`
- Popup prompt asking for username:password with `<getsn>`
- Copy input string from `0x2400` to `0x3c6e` using `<strcpy>`
- Test if the password is valid with `<test_password_valid>`
- Reprint our input to screen using `<printf>`
- Add some `0xa` to the stack frame with `<putchar>`
- If the bytes at `0x3c6c` is zero, print that the password is incorrect.
- If the bytes at `0x3c6c` is not zero, unlock the door.

Well, with this information, let's check the possibilities:


### How long the input goes

As it was said, we are limited to `0x13` characters of input (or 19 characters in decimal).

Maybe we can use the return at `<main>` to redirect the return to the `<unlock_door>` function. This will make a common solution, but maybe it will work.

But, there's a problem. If we check the `<main>` function, no return call is made, it just "bleeds" to the next function `<__stop_progExec__>`, making the program halt. So, this is not a possibility.


### The Odd in the house : printf

This is an uncommon function. Most of the challenges before this one, it was used `<puts>` to print the strings and information, so why change it?

This makes me think that the solution is on this function. This is a common/famous function in the C library, so we are dealing with a C compilation? Let's check the full `printf` function:

```
45c8 <printf>

45c8:  0b12           push	r11
45ca:  0a12           push	r10
45cc:  0912           push	r9
45ce:  0812           push	r8
45d0:  0712           push	r7
45d2:  0412           push	r4
45d4:  0441           mov	sp, r4
45d6:  3450 0c00      add	#0xc, r4
45da:  2183           decd	sp
45dc:  1b44 0200      mov	0x2(r4), r11
45e0:  8441 f2ff      mov	sp, -0xe(r4)
45e4:  0f4b           mov	r11, r15
45e6:  0e43           clr	r14
45e8:  0b3c           jmp	$+0x18 <printf+0x38>
45ea:  1f53           inc	r15
45ec:  7d90 2500      cmp.b	#0x25, r13
45f0:  0720           jnz	$+0x10 <printf+0x38>
45f2:  6d9f           cmp.b	@r15, r13
45f4:  0320           jnz	$+0x8 <printf+0x34>
45f6:  1f53           inc	r15
45f8:  0d43           clr	r13
45fa:  013c           jmp	$+0x4 <printf+0x36>
45fc:  1d43           mov	#0x1, r13
45fe:  0e5d           add	r13, r14
4600:  6d4f           mov.b	@r15, r13
4602:  4d93           tst.b	r13
4604:  f223           jnz	$-0x1a <printf+0x22>
4606:  0f4e           mov	r14, r15
4608:  0f5f           add	r15, r15
460a:  2f53           incd	r15
460c:  018f           sub	r15, sp
460e:  0941           mov	sp, r9
4610:  0c44           mov	r4, r12
4612:  2c52           add	#0x4, r12
4614:  0f41           mov	sp, r15
4616:  0d43           clr	r13
4618:  053c           jmp	$+0xc <printf+0x5c>
461a:  af4c 0000      mov	@r12, 0x0(r15)
461e:  1d53           inc	r13
4620:  2f53           incd	r15
4622:  2c53           incd	r12
4624:  0d9e           cmp	r14, r13
4626:  f93b           jl	$-0xc <printf+0x52>
4628:  0a43           clr	r10
462a:  3740 0900      mov	#0x9, r7
462e:  4a3c           jmp	$+0x96 <printf+0xfc>
4630:  084b           mov	r11, r8
4632:  1853           inc	r8
4634:  7f90 2500      cmp.b	#0x25, r15
4638:  0624           jz	$+0xe <printf+0x7e>
463a:  1a53           inc	r10
463c:  0b48           mov	r8, r11
463e:  8f11           sxt	r15
4640:  b012 5045      call	#0x4550 <putchar>
4644:  3f3c           jmp	$+0x80 <printf+0xfc>
4646:  6e48           mov.b	@r8, r14
4648:  4e9f           cmp.b	r15, r14
464a:  0620           jnz	$+0xe <printf+0x90>
464c:  1a53           inc	r10
464e:  3f40 2500      mov	#0x25, r15
4652:  b012 5045      call	#0x4550 <putchar>
4656:  333c           jmp	$+0x68 <printf+0xf6>
4658:  7e90 7300      cmp.b	#0x73, r14
465c:  0b20           jnz	$+0x18 <printf+0xac>
465e:  2b49           mov	@r9, r11
4660:  053c           jmp	$+0xc <printf+0xa4>
4662:  1a53           inc	r10
4664:  1b53           inc	r11
4666:  8f11           sxt	r15
4668:  b012 5045      call	#0x4550 <putchar>
466c:  6f4b           mov.b	@r11, r15
466e:  4f93           tst.b	r15
4670:  f823           jnz	$-0xe <printf+0x9a>
4672:  253c           jmp	$+0x4c <printf+0xf6>
4674:  7e90 7800      cmp.b	#0x78, r14
4678:  1c20           jnz	$+0x3a <printf+0xea>
467a:  2b49           mov	@r9, r11
467c:  173c           jmp	$+0x30 <printf+0xe4>
467e:  0f4b           mov	r11, r15
4680:  8f10           swpb	r15
4682:  3ff0 ff00      and	#0xff, r15
4686:  12c3           clrc
4688:  0f10           rrc	r15
468a:  0f11           rra	r15
468c:  0f11           rra	r15
468e:  0f11           rra	r15
4690:  1a53           inc	r10
4692:  079f           cmp	r15, r7
4694:  0338           jl	$+0x8 <printf+0xd4>
4696:  3f50 3000      add	#0x30, r15
469a:  023c           jmp	$+0x6 <printf+0xd8>
469c:  3f50 5700      add	#0x57, r15
46a0:  b012 5045      call	#0x4550 <putchar>
46a4:  0b5b           add	r11, r11
46a6:  0b5b           add	r11, r11
46a8:  0b5b           add	r11, r11
46aa:  0b5b           add	r11, r11
46ac:  0b93           tst	r11
46ae:  e723           jnz	$-0x30 <printf+0xb6>
46b0:  063c           jmp	$+0xe <printf+0xf6>
46b2:  7e90 6e00      cmp.b	#0x6e, r14
46b6:  0320           jnz	$+0x8 <printf+0xf6>
46b8:  2f49           mov	@r9, r15
46ba:  8f4a 0000      mov	r10, 0x0(r15)
46be:  2953           incd	r9
46c0:  0b48           mov	r8, r11
46c2:  1b53           inc	r11
46c4:  6f4b           mov.b	@r11, r15
46c6:  4f93           tst.b	r15
46c8:  b323           jnz	$-0x98 <printf+0x68>
46ca:  1144 f2ff      mov	-0xe(r4), sp
46ce:  2153           incd	sp
46d0:  3441           pop	r4
46d2:  3741           pop	r7
46d4:  3841           pop	r8
46d6:  3941           pop	r9
46d8:  3a41           pop	r10
46da:  3b41           pop	r11
46dc:  3041           ret
```

This is a long function and kinda complex (boring), because it jumps from one place to another with some checks and others without checks.

So, how are we going to analyse this function? Well, first we are going to check the ramifications of this function, in another words, let's list the "Compare" (`cmp`) lines.

- `cmp.b	#0x25, r13` => Compares r13 with literal `0x25` (%)
- `cmp.b	@r15, r13` => Compares r13 with @r15
- `cmp		r14, r13` => Compares r13 with r14
- `cmp.b	#0x25, r15` => Compares r15 with literal `0x25` (%)
- `cmp.b	r15, r14` => Compares r15 with r14
- `cmp.b	#0x73, r14` => Compares r14 with literal `0x73` (s)
- `cmp.b	#0x78, r14` => Compares r14 with literal `0x78` (x)
- `cmp		r15, r7` => Compares r15 with r7
- `cmp.b	#0x6e, r14` => Compares r14 with literal `0x6e` (n)

We can start checking the literals ASCII characters, those are `%`, `s`, `x`, `n`. As we talked before, these are common terms for C developers.


### Specifier Characters

You can check the details in the documentation [Here](https://cplusplus.com/reference/cstdio/printf/), but we are dealing with:

- `%s` : String of Characters
- `%x` : Unsigned hexadecimal integer
- `%n` : Nothing is printed and the number of characters written so far is stored in the pointed location

So, making an example with `%s` :

```c
printf ("%s text here \n", "A string");
//       |                 |________|
//       +----------------------

// output : "A string text here"
```

We get the position of an argument, and replace into the original string. This works as well with `%x` :

```c
printf ("100 decimal in hex : %x", 100);

// output : "100 decimal in hex : 64"
```

This one takes an information and read as hexadecimal value. For last the `%n` specifier:

```c
// create an int variable
int val;

// half A and half B, store %n into the variable val
printf("AA AA A %n BBBBBBBBBBBBB\n", &val);

// print the value of val
printf("val = %d \n", val);

// output : "val = 8"
```

The output is 8 because `%n` is taking the length of characters before the invocation. It's counting everything before it ("AA AA A ") and storing into `val`, making it equals 8. Everything after `%n` is ignored, so no counting for the 'B' section.

Knowing that, let's make some experiments.

I tried to make some random inputs, something like:

```
AAAAAA%s%sAAA
AAA%x%x%x%x%x
%x%s%n%s%x
```

And so on, until I tried:

```
AA%x%n%n%n
```

This popped an error ("load address unaligned: 4141") and the program halted in the middle of `<printf>`. This was interesting, my assumptions was right, it was something about `printf`. I didn't know much about C, so I looked for vulnerabilities in `printf` function. Here's what I found at [OWASP](https://owasp.org/www-community/attacks/Format_string_attack) website:

```
Below are some format parameters which can be used and their consequences:
•"%x" Read data from the stack
•"%s" Read character strings from the process' memory
•"%n" Write an integer to locations in the process’ memory
```

This made me think, what if I used this to manipulate the stack and memory?

### Modifying Memory and %x%n


At the start of `<printf>` function, the registers `r11`, `r10`, `r9`, `r8`, `r7` and `r4` values are being pushed to the stack, these values will be used afterwards, maybe some input can manipulate the outcome. 

What we can see with our last input (`AA%x%n%n%n`) the value unaligned was "4141", which is, in ASCII, "AA". So it just tried to modify the memory value at `0x4141`. Ok, maybe with that we can modify some value.

Remember, to call the `<unlock_door>` function, the bytes at `0x3c6c` must not be zero. Maybe we can modify this address? Let's try with the following input:

```
l<%x%n%n%n
```

The `l<` is a representation of `0x6c` and `0x3c` bytes from the ASCII table, which is our target address corrected for endianness.

With that, the value at `0x3c6c` is not zero anymore, making the door open.

I tried to clean the input, so the final input I used to open the door was:

```
l<%x%n
```

