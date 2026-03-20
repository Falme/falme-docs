---
title: "Whitehorse"
weight: 6
# bookFlatSection: false
# bookToc: true
# bookHidden: true
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Microcorruption - Whitehorse

Microcorruption gives us a debugger and a password to unlock a device, our job is to find the password reading through assembly code and using some reverse engineering tricks. Each challenge is named after a city, each one more difficult than the previous one.

If you didn't solve the challenge for yourself, I recommend that you stop reading, solve the challenge and check my solution afterwards, do not spoil the challenge and have fun.

## How to solve it

First of all, let's check the `main` function:

```
4438 <main>
4438:  b012 f444      call	#0x44f4 <login>
```

The only thing it does is to call the `login` function, so let's check that too:

```
44f4 <login>
44f4:  3150 f0ff      add	#0xfff0, sp
44f8:  3f40 7044      mov	#0x4470 "Enter the password to continue.", r15
44fc:  b012 9645      call	#0x4596 <puts>
4500:  3f40 9044      mov	#0x4490 "Remember: passwords are between 8 and 16 characters.", r15
4504:  b012 9645      call	#0x4596 <puts>
4508:  3e40 3000      mov	#0x30, r14
450c:  0f41           mov	sp, r15
450e:  b012 8645      call	#0x4586 <getsn>
4512:  0f41           mov	sp, r15
4514:  b012 4644      call	#0x4446 <conditional_unlock_door>
4518:  0f93           tst	r15
451a:  0324           jz	$+0x8 <login+0x2e>
451c:  3f40 c544      mov	#0x44c5 "Access granted.", r15
4520:  023c           jmp	$+0x6 <login+0x32>
4522:  3f40 d544      mov	#0x44d5 "That password is not correct.", r15
4526:  b012 9645      call	#0x4596 <puts>
452a:  3150 1000      add	#0x10, sp
452e:  3041           ret
```

So in a few words, this function:
- Write on screen asking for a password with `<puts>`
- Ask for a password input with `<getsn>`
- Make a check if the password is correct with `<conditional_unlock_door>`
- If the password is correct, write that the password is correct
- If the password is not correct, write that the password is not correct

After all that, we can also see that at line `0x452a` there's an `stack pointer` add, maybe we could use that. But first let's check the `conditional_unlock_door` function:

```
4446 <conditional_unlock_door>
4446:  0412           push	r4
4448:  0441           mov	sp, r4
444a:  2453           incd	r4
444c:  2183           decd	sp
444e:  c443 fcff      mov.b	#0x0, -0x4(r4)
4452:  3e40 fcff      mov	#0xfffc, r14
4456:  0e54           add	r4, r14
4458:  0e12           push	r14
445a:  0f12           push	r15
445c:  3012 7e00      push	#0x7e
4460:  b012 3245      call	#0x4532 <INT>
4464:  5f44 fcff      mov.b	-0x4(r4), r15
4468:  8f11           sxt	r15
446a:  3152           add	#0x8, sp
446c:  3441           pop	r4
446e:  3041           ret
```

This function, mostly, just send values around pointers with `sp` (stack pointer), then, send a `0x7e` to the stack and call `<INT>`:

```
4532 <INT>
4532:  1e41 0200      mov	0x2(sp), r14
4536:  0212           push	sr
4538:  0f4e           mov	r14, r15
453a:  8f10           swpb	r15
453c:  024f           mov	r15, sr
453e:  32d0 0080      bis	#0x8000, sr
4542:  b012 1000      call	#0x10
4546:  3241           pop	sr
4548:  3041           ret
```

The `INT` function read 2 bytes after the `sp` (stack pointer) and send it to `r14`, this is moved around with `r15` to then make a `bis` action with `0x8000` and `sr`. Just think about the `bis` command as an `or` command (`0x8000` | `sr`).

Then, it will call the function at `0x10` (`<__trap_interrupt>`) to see if the door open or not.

> **Little Intermission : How exactly the door opens**
>  
> The trap_interrupt (`<__trap_interrupt>`) function deals with the hardware, it sends a message through the `sr` pointer to check what the hardware will do with the operating system.
>  
> In our case, the value `ff00` is necessary to be set at `sr` for the door to be opened, even if the function `<__trap_interrupt>` is called, it will not open with other values.

Running our code with a random password, we can see that the value of `sr` at the check function is always `0xfe00`, close to what we need, but no right yet, we must need to change this value.

If we are going to change some values, first let's check how long our buffer overflow input reach in the memory. Let's try adding a bunch of `6`'s.

```
666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666
```

After inputting and making a step command (`s`), we can see that our input reach between `0x3cb8` and `0x3ce7`. 

Now let's see if the command at `login` at line `0x452a` and `0x452e` interlaps with our input, those lines are:

```
452a:  3150 1000      add	#0x10, sp
452e:  3041           ret
```

This lines add `0x10` to the `sp` and then use the `sp` position to make a return to the function that called, we must see if this position in `sp` called by return (`ret`) is modifiable by our input.

To do that:
- Add a breakpoint at `0x452a`, and reach it 
- Check the position of the `sp` in the memory
- If the position is between `0x3cb8` and `0x3ce7`, we can change the return address.

For our luck, the position of return is at `0x3cc8`, a position that our input can change it.

With that information, let's modify the return to another place, let's move to `0x451c`. After some testing, our input will look like:

```
666666666666666666666666666666661c45
```

This input will make the jump to the line `0x451c`, the "Access Granted" text. Making this jump, our door is still not unlocked. This time, we will try to make the return to the `<INT>` function, maybe running this function twice will make it better for us to manipulate the flow of the program. Our next input is:

```
666666666666666666666666666666663245
```

After redirecting to the address `0x4532` (`<INT>`), the stack will be storing even further, making the value in the `r14` and `r15` be read again, but now at a position that our input can manipulate. This values will be added to the `sr` pointer, so maybe we can make the `sr` to be `0xff00` to unlock the door.

The second read happens 3 bytes after the last return value that we modified. Because of that, we will add 2 zero bytes (`0000`) to our input and then add the unlock value to the `sr` (`0xff00`). The input will be something like:

```
6666666666666666666666666666666632450000ff00
```

With that, we return again to the `<INT>` function, add more values to the stack, modify the `sr` to be `0xff00` to unlock the door.
