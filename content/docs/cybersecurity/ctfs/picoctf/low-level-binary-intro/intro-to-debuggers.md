---
title: "Intro to Debuggers"
weight: 6
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# PicoCTF: Low Level Binary Intro - Intro to Debuggers

This [PicoCTF Playlist](https://play.picoctf.org/playlists/2) section is called Intro to Debuggers, the challenges will go deeper into how to use the Debugger, specifically GDB, a common debugger and disassembler. After some explanations of how to use the GDB, we can do the challenges.

I am also using a Virtual Machine with Ubuntu to solve these challenges.

---

## GDB baby step 1

> Can you figure out what is in the eax register at the end of the main function? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}. Disassemble this (file).

So what we need to do is:

1. Get the file (debugger0_a) to disassemble
2. Disassemble the main function with GDB
3. Gather the value of eax register at the end of main function

In summary, we need to gather a assembly dump from the requested file, just like in the previous challenges.

So after downloaded the file, we can disassemble the main function with the following command in shell:

```shell
chmod +x ./debugger0_a
gdb ./debugger0_a
```

and inside gdb:

```shell
(gdb) disassemble main
```

Making these actions, gdb returns with the following dump:

```python
0x0000000000001129 <+0>:	endbr64
0x000000000000112d <+4>:	push   %rbp
0x000000000000112e <+5>:	mov    %rsp,%rbp
0x0000000000001131 <+8>:	mov    %edi,-0x4(%rbp)
0x0000000000001134 <+11>:	mov    %rsi,-0x10(%rbp)
0x0000000000001138 <+15>:	mov    $0x86342,%eax
0x000000000000113d <+20>:	pop    %rbp
0x000000000000113e <+21>:	ret
```

It's kinda hard for me to read this one, maybe if we change to we are used to, the intel syntax, for that we will:

```shell
(gdb) set disassembly-flavor intel
(gdb) disassemble main
```

and that returns the following dump:

```python
0x0000000000001129 <+0>:	endbr64
0x000000000000112d <+4>:	push   rbp
0x000000000000112e <+5>:	mov    rbp,rsp
0x0000000000001131 <+8>:	mov    DWORD PTR [rbp-0x4],edi
0x0000000000001134 <+11>:	mov    QWORD PTR [rbp-0x10],rsi
0x0000000000001138 <+15>:	mov    eax,0x86342
0x000000000000113d <+20>:	pop    rbp
0x000000000000113e <+21>:	ret
```

Really better, now, we need to find the final result to EAX register. The funny part is that there's only one command that affects EAX:

```python
0x0000000000001138 <+15>:	mov    eax,0x86342
```

So, the value of EAX is `0x86342`.

But we actually need the decimal value, so we put it in our python script [HexToDec.py](https://github.com/Falme-SideProjects/python-hex-to-dec/blob/main/HexToDec.py), then we can have the answer:

```python
python3 HexToDec.py 0x86342
```

We take the result with picoCTF{value} and we have the flag:

{{% details title="Answer: Flag 1" open=false %}}
```
picoCTF{549698}
```
{{% /details %}}

---

## GDB baby step 2

> Can you figure out what is in the eax register at the end of the main function? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}. Debug this (file).

The description is the same as the last one, but this time, we are playing with breakpoints and loops in the program. The exercise is to start looking into dynamic analysis.

Static analysis is what we have done before, looking through code to understand what it does. Dynamic analysis study the code through execution and debugging (breakpoints).

Doing the same assembly dump as the previous exercise, we gather this instruction:

```python
0x0000000000401106 <+0>:	endbr64
0x000000000040110a <+4>:	push   rbp
0x000000000040110b <+5>:	mov    rbp,rsp
0x000000000040110e <+8>:	mov    DWORD PTR [rbp-0x14],edi
0x0000000000401111 <+11>:	mov    QWORD PTR [rbp-0x20],rsi
0x0000000000401115 <+15>:	mov    DWORD PTR [rbp-0x4],0x1e0da
0x000000000040111c <+22>:	mov    DWORD PTR [rbp-0xc],0x25f
0x0000000000401123 <+29>:	mov    DWORD PTR [rbp-0x8],0x0
0x000000000040112a <+36>:	jmp    0x401136 <main+48>
0x000000000040112c <+38>:	mov    eax,DWORD PTR [rbp-0x8]
0x000000000040112f <+41>:	add    DWORD PTR [rbp-0x4],eax
0x0000000000401132 <+44>:	add    DWORD PTR [rbp-0x8],0x1
0x0000000000401136 <+48>:	mov    eax,DWORD PTR [rbp-0x8]
0x0000000000401139 <+51>:	cmp    eax,DWORD PTR [rbp-0xc]
0x000000000040113c <+54>:	jl     0x40112c <main+38>
0x000000000040113e <+56>:	mov    eax,DWORD PTR [rbp-0x4]
0x0000000000401141 <+59>:	pop    rbp
0x0000000000401142 <+60>:	ret
```

We can see 2 problems that we not faced before.

1. EAX register is being called in multiple places
2. we have JUMPs in the instruction, indicating a possible loop

The loop can be found as the following pattern:

At main+51 we have a comparison:

```python
0x0000000000401139 <+51>:	cmp    eax,DWORD PTR [rbp-0xc]
```

If this comparison is less than a value, jump to another line; JL (Jump if Less)
So in this case, we are checking if `[rpb-0xc]` is less than `EAX` 

If so, we jump to the line main+38, so we go backwards and do it again. That way we find a loop

```python
0x000000000040113c <+54>:	jl     0x40112c <main+38>
```

So, with that information, we can assure that we will not count every single time that it loops and add a value to EAX.

In fact, we will go to the end of the program, and just read what EAX is, like a good dynamic analysis want us to do.

First, we add a breakpoint to the end of the main function before the return. the `main+59` line is a good place to do this. So we go to gdb and do the following:

```shell
chmod +x ./debugger0_b
gdb ./debugger0_b
```

and inside gdb we set up a breakpoint, where it will pause the execution:

```shell
(gdb) break *main+59
```

And run the program normally through the GDB debugger:

```shell
(gdb) run
```

After a little moment, it will stop with the following message:

```python
Breakpoint 1, 0x0000000000401141 in main ()
```

and now the program is paused as we asked on line `main+59` , you can check the address, is the same number. Now we can ask for the value of EAX, that will show the result for the flag in hexadecimal and decimal.

```shell
(gdb) info registers eax
```

We take the result with picoCTF{value} and we have the flag:

{{% details title="Answer:Flag 2" open=false %}}
```
picoCTF{307019}
```
{{% /details %}}

---

## GDB baby step 3

> Now for something a little different. 0x2262c96b is loaded into memory in the main function. Examine byte-wise the memory that the constant is loaded in by using the GDB command x/4xb addr. The flag is the four bytes as they are stored in memory. If you find the bytes 0x11 0x22 0x33 0x44 in the memory location, your flag would be: picoCTF{0x11223344}. Debug this (file).

After a brief explanation about how to read Memory in GDB, the exercise is presented.

We need to find the bytes in the memory address of the file `debugger0_c` . The tip is the value `0x2262c96b` in the main function.

So first of all, we need to disassemble the main function, same as before, resulting in the following assembly code:

```python
0x0000000000401106 <+0>:	endbr64
0x000000000040110a <+4>:	push   rbp
0x000000000040110b <+5>:	mov    rbp,rsp
0x000000000040110e <+8>:	mov    DWORD PTR [rbp-0x14],edi
0x0000000000401111 <+11>:	mov    QWORD PTR [rbp-0x20],rsi
0x0000000000401115 <+15>:	mov    DWORD PTR [rbp-0x4],0x2262c96b
0x000000000040111c <+22>:	mov    eax,DWORD PTR [rbp-0x4]
0x000000000040111f <+25>:	pop    rbp
0x0000000000401120 <+26>:	ret
```

following this, the value `0x2262c96b` is moved (MOV at +15) to the memory address $RBP-0x4.

First some explanations. RBP register is the Register Base Pointer, it point to the base of the Stack Frame, and the addition of the values is through the negative numbering.

So usually we go for something like RBP-0x4 or RBP-0x8, this is adding values from the RBP (Register Base Pointer) to the RSP (Register Stack Pointer).

So we are moving the value `0x2262c96b` to the Register Base Pointer position Minus 4.

Add a breakpoint at main+25 and run the program:

```shell
(gdb) break *main+25
(gdb) run
```

And call the values inside RBP-0x4. where `x/` is the command to call the memory reading, we want 4 bytes (4) in hexadecimal (x) each with byte-sized (b) resulting the command `4xb` .

```shell
(gdb) x/4xb $rbp-0x4
```

Resulting in:

```python
0x7fffffffddbc:	0x6b	0xc9	0x62	0x22
```

Well, actually the value returned is inverted from the original data `0x2262c96b` . It's because we are dealing with Little endian.

We have Big Endian and Small Endian. Will add two images from a video (no longer listed) from C3rb3ru5d3d53c explaining this visually:

### Big Endian

Normally "correct" the way we read the hexadecimal values.

Where

 - M : is the Most Significant Byte
 - L : is the Least Significant Byte

![Big Endian](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/213j4ywj4v2ywog92yxm.png)

### Little Endian

Normally "inverted" the way we read the hexadecimal values.

![Little Endian](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/o4lslhjanoamrau4wo4l.png)


So the exercise asked for us to write down the flag as we read them, so, as Little endian is on the screen.

simply an exercise to just read a value in memory and understand Little endian.


We take the result with picoCTF{value} and we have the flag:

{{% details title="Answer:Flag 3" open=false %}}
```
picoCTF{0x6bc96222}
```
{{% /details %}}

---

## GDB baby step 4

> main calls a function that multiplies eax by a constant. The flag for this challenge is that constant in decimal base. If the constant you find is 0x1000, the flag will be picoCTF{4096}. Debug this (file).

This exercise is very simple, is to show that we can call and disassemble many functions, not only main. we need to find a constant number that multiplies with EAX.

So me make the same as the other exercises to disassemble the main function returning the following:

```python
0x000000000040111c <+0>:	endbr64
0x0000000000401120 <+4>:	push   rbp
0x0000000000401121 <+5>:	mov    rbp,rsp
0x0000000000401124 <+8>:	sub    rsp,0x20
0x0000000000401128 <+12>:	mov    DWORD PTR [rbp-0x14],edi
0x000000000040112b <+15>:	mov    QWORD PTR [rbp-0x20],rsi
0x000000000040112f <+19>:	mov    DWORD PTR [rbp-0x4],0x28e
0x0000000000401136 <+26>:	mov    DWORD PTR [rbp-0x8],0x0
0x000000000040113d <+33>:	mov    eax,DWORD PTR [rbp-0x4]
0x0000000000401140 <+36>:	mov    edi,eax
0x0000000000401142 <+38>:	call   0x401106 <func1>
0x0000000000401147 <+43>:	mov    DWORD PTR [rbp-0x8],eax
0x000000000040114a <+46>:	mov    eax,DWORD PTR [rbp-0x4]
0x000000000040114d <+49>:	leave
0x000000000040114e <+50>:	ret
```

The line main+38 calls another function called "func1". Maybe the multiplication is there. Lets check it out

```shell
(gdb) disassemble func1
```

Returns us the following:

```python
0x0000000000401106 <+0>:	endbr64
0x000000000040110a <+4>:	push   rbp
0x000000000040110b <+5>:	mov    rbp,rsp
0x000000000040110e <+8>:	mov    DWORD PTR [rbp-0x4],edi
0x0000000000401111 <+11>:	mov    eax,DWORD PTR [rbp-0x4]
0x0000000000401114 <+14>:	imul   eax,eax,0x3269
0x000000000040111a <+20>:	pop    rbp
0x000000000040111b <+21>:	ret
```

At the line func+14 we have a multiplication with EAX, maybe the value 0x3269 is what we need.

But we actually need the decimal value, so we put it in our python script [HexToDec.py](https://github.com/Falme-SideProjects/python-hex-to-dec/blob/main/HexToDec.py), then we can have the answer:

```python
python3 HexToDec.py 0x3269
```

We take the result with picoCTF{value} and we have the flag:


{{% details title="Answer: Flag 4" open=false %}}
```
picoCTF{12905}
```
{{% /details %}}

---

## ASCII FTW

> This program has constructed the flag using hex ascii values. Identify the flag text by disassembling the program. You can download the file from here.

This is the challenge of the module. The mission is to find the flag with GDB in bytes, and then convert to ASCII/String.

First, let's look into the main function:

```python
0x0000555555555169 <+0>:	endbr64
0x000055555555516d <+4>:	push   rbp
0x000055555555516e <+5>:	mov    rbp,rsp
0x0000555555555171 <+8>:	sub    rsp,0x30
0x0000555555555175 <+12>:	mov    rax,QWORD PTR fs:0x28
0x000055555555517e <+21>:	mov    QWORD PTR [rbp-0x8],rax
0x0000555555555182 <+25>:	xor    eax,eax
0x0000555555555184 <+27>:	mov    BYTE PTR [rbp-0x30],0x70
0x0000555555555188 <+31>:	mov    BYTE PTR [rbp-0x2f],0x69
0x000055555555518c <+35>:	mov    BYTE PTR [rbp-0x2e],0x63
0x0000555555555190 <+39>:	mov    BYTE PTR [rbp-0x2d],0x6f
0x0000555555555194 <+43>:	mov    BYTE PTR [rbp-0x2c],0x43
0x0000555555555198 <+47>:	mov    BYTE PTR [rbp-0x2b],0x54
0x000055555555519c <+51>:	mov    BYTE PTR [rbp-0x2a],0x46
0x00005555555551a0 <+55>:	mov    BYTE PTR [rbp-0x29],0x7b
0x00005555555551a4 <+59>:	mov    BYTE PTR [rbp-0x28],0x41
0x00005555555551a8 <+63>:	mov    BYTE PTR [rbp-0x27],0x53
0x00005555555551ac <+67>:	mov    BYTE PTR [rbp-0x26],0x43
0x00005555555551b0 <+71>:	mov    BYTE PTR [rbp-0x25],0x49
0x00005555555551b4 <+75>:	mov    BYTE PTR [rbp-0x24],0x49
0x00005555555551b8 <+79>:	mov    BYTE PTR [rbp-0x23],0x5f
0x00005555555551bc <+83>:	mov    BYTE PTR [rbp-0x22],0x49
0x00005555555551c0 <+87>:	mov    BYTE PTR [rbp-0x21],0x53
0x00005555555551c4 <+91>:	mov    BYTE PTR [rbp-0x20],0x5f
0x00005555555551c8 <+95>:	mov    BYTE PTR [rbp-0x1f],0x45
0x00005555555551cc <+99>:	mov    BYTE PTR [rbp-0x1e],0x41
0x00005555555551d0 <+103>:	mov    BYTE PTR [rbp-0x1d],0x53
0x00005555555551d4 <+107>:	mov    BYTE PTR [rbp-0x1c],0x59
0x00005555555551d8 <+111>:	mov    BYTE PTR [rbp-0x1b],0x5f
0x00005555555551dc <+115>:	mov    BYTE PTR [rbp-0x1a],0x38
0x00005555555551e0 <+119>:	mov    BYTE PTR [rbp-0x19],0x39
0x00005555555551e4 <+123>:	mov    BYTE PTR [rbp-0x18],0x36
0x00005555555551e8 <+127>:	mov    BYTE PTR [rbp-0x17],0x30
0x00005555555551ec <+131>:	mov    BYTE PTR [rbp-0x16],0x46
0x00005555555551f0 <+135>:	mov    BYTE PTR [rbp-0x15],0x30
0x00005555555551f4 <+139>:	mov    BYTE PTR [rbp-0x14],0x41
0x00005555555551f8 <+143>:	mov    BYTE PTR [rbp-0x13],0x46
0x00005555555551fc <+147>:	mov    BYTE PTR [rbp-0x12],0x7d
0x0000555555555200 <+151>:	movzx  eax,BYTE PTR [rbp-0x30]
0x0000555555555204 <+155>:	movsx  eax,al
0x0000555555555207 <+158>:	mov    esi,eax
0x0000555555555209 <+160>:	lea    rdi,[rip+0xdf4]        # 0x555555556004
0x0000555555555210 <+167>:	mov    eax,0x0
0x0000555555555215 <+172>:	call   0x555555555070 <printf@plt>
0x000055555555521a <+177>:	nop
0x000055555555521b <+178>:	mov    rax,QWORD PTR [rbp-0x8]
0x000055555555521f <+182>:	xor    rax,QWORD PTR fs:0x28
0x0000555555555228 <+191>:	je     0x55555555522f <main+198>
0x000055555555522a <+193>:	call   0x555555555060 <__stack_chk_fail@plt>
0x000055555555522f <+198>:	leave
0x0000555555555230 <+199>:	ret
```

We have many addition of bytes into the memory from main+27 (RBP-0x30) to main+147 (RBP-0x12).

So we can add a breakpoint at somewhere like main+155, just after the values was added to memory.

```shell
(gdb) break *main+155
(gdb) run
```

And then read the memory from the RBP-0x30.

So the question is, how we read and why start from -0x30.

Because this negative position, we are incrementing the values to read, so making the reading start from RBP-0x30 it will goes after to -0x2f, then -0x2e, etc...

and to read the string we will change the type from byte (x) to string (s). Same as the last one we've done, but changing this we got

```shell
(gdb) x/1sb $rbp-0x30
```

Another this is, we usually read a lot of bytes, but in this case we do `1sb`, we are reading 1 string. This is kinda weird in a length sense, but this will output each "String" and not "Character" from the memory.

Reading this value from memory we receive directly the flag to put into the answer.

{{% details title="Answer: Flag 5" open=false %}}
```
picoCTF{ASCII_IS_EASY_8960F0AF}
```
{{% /details %}}
