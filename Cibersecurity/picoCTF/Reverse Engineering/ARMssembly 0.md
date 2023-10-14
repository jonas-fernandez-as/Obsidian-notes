

#### Description

What integer does this program print with arguments `4134207980` and `950176538`? File: [chall.S](https://mercury.picoctf.net/static/da36e19990a2cede1dff10f9f33fe4b4/chall.S) Flag format: picoCTF{XXXXXXXX} -> (hex, lowercase, no 0x, and 32 bits. ex. 5614267 would be picoCTF{0055aabb})

#### Hints

Simple compare
### Solution

In this challenge I helped myself with the IA , basiccally the IA told me that the  first portion

```arm
sub sp, sp, #16 
```
Is selecting two spaces (sp) from the memory

```arm
 str w0, [sp, 12]  
 str w1, [sp, 8]  
```
This second step (str) is ponting and storing two spaces from the memory

```
    ldr w1, [sp, 12] 
    ldr w0, [sp, 8] 
```
And this (ldr) is loading this two spaces

```
cmp w1, w0
```
cmp is comparing two values


```
.L2:
        ldr     w0, [sp, 8]
.L3:
        add     sp, sp, 16
        ret
        .size   func1, .-func1
        .section        .rodata
        .align  3

```
At `.L2` we load a value from the `stack at offset 8` into the variable w0 and continue execution back in `func1`. In `.L3` we just `add` 16 back to `sp`, ie. we fill the stack back again.

Then we have this portion of code:
```
.LC0:
        .string "Result: %ld\n"
        .text
        .align  2
        .global main
        .type   main, %function

```
And then seems like is sending the result of this compared numbers to the main function. 

Asking to the IA told me that is transforming the number to hex ... so the solution for me was this

On python I transform the number to 32 bit hex, removing the 0x. 

```
hex_number = hex(4134207980)
print(hex_number)
```

If you have problems try with the two numbers `4134207980` and `950176538`. Finally you will get the flag.


 `picoCTF{hexvalue})`
