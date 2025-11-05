# Challenge 1 : GDB baby step 1

> Can you figure out what is in the eax register at the end of the main function? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}. Disassemble this.

## Solution

As the challenge name suggests this challenge involved using gdb to disassemble the given file, so i fired up gdb using kali, and ran the following :
```shell
┌──(kali㉿kali)-[~/Desktop/VM_Files]
└─$ gdb debugger0_a
GNU gdb (Debian 16.3-1) 16.3
Copyright (C) 2024 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from debugger0_a...
(No debugging symbols found in debugger0_a)
(gdb) disas main
Dump of assembler code for function main:
   0x0000000000001129 <+0>:     endbr64
   0x000000000000112d <+4>:     push   %rbp
   0x000000000000112e <+5>:     mov    %rsp,%rbp
   0x0000000000001131 <+8>:     mov    %edi,-0x4(%rbp)
   0x0000000000001134 <+11>:    mov    %rsi,-0x10(%rbp)
   0x0000000000001138 <+15>:    mov    $0x86342,%eax
   0x000000000000113d <+20>:    pop    %rbp
   0x000000000000113e <+21>:    ret
End of assembler dump.
(gdb) 
```
As soon as i deassembled the file, i saw `%eax` registry at the end of the assembler code, next to it was written `0x86342`, as we have already been told the flag is in eax register in hex, so i know i had found it, i just have to convert it into decimal and put it inside `picoCTF{}` to get the complete flag and complete this challenge.

## Flag
```
picoCTF{549698}
```

# Challenge 2 : ARMssembly 1
>For what argument does this program print `win` with variables 85, 6 and 3? File: chall_1.S Flag format: picoCTF{XXXXXXXX} -> (hex, lowercase, no 0x, and 32 bits. ex. 5614267 would be picoCTF{0055aabb})

# Solution : 

In the file given i see the main function takes the cli input from us and then passes it as an argument to `func` function by converting it from string to an integer, then i see that `func` returns a value which it compares to 0 if they are true it returns with `WIN`, which is the end goal for this challenge, so now onwards to analyze the `func` function to find the value which will get 0 returned, so for that I saw that the number 85, 6, 3 being stored and left shift and division being performed on them. So i just did that to get the number 1813.

```
func:
	sub	sp, sp, #32
	str	w0, [sp, 12]
	mov	w0, 85
	str	w0, [sp, 16]
	mov	w0, 6
	str	w0, [sp, 20]
	mov	w0, 3
	str	w0, [sp, 24]
	ldr	w0, [sp, 20]
	ldr	w1, [sp, 16]
	lsl	w0, w1, w0
	str	w0, [sp, 28]
	ldr	w1, [sp, 28]
	ldr	w0, [sp, 24]
	sdiv	w0, w1, w0
	str	w0, [sp, 28]
	ldr	w1, [sp, 28]
	ldr	w0, [sp, 12]
	sub	w0, w1, w0
	str	w0, [sp, 28]
	ldr	w0, [sp, 28]
	add	sp, sp, 32
	ret
```
the number i then converted to hexadecimal to and padded the left side with zero for 32 bit and enclosed within `picoCTF{}` to get the flag and complete the challenge.

## Flag : 
```
picoCTF{00000715}
```
