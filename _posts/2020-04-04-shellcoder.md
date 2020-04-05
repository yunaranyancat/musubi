---
layout: post
title: "Path to become a shellcoder"
categories: jekyll
permalink : /paths/shellcoder
---

Hey , I'm writing this journal that will be updated periodically at the end of the day as I progress through the Shellcoder's Handbook 2nd Edition and other shellcoding related things. I will write anything that I found useful however, do note that this is more of a journal/diary rather than a complete technical writeup. Let's go..

### 4 April 2020

Went through the first few pages of Chapter 2.

Seems like I need to install some OS so I can follow along.

Tried to install Debian 3.1r4 but seems like I suck at installing old OS.

Guess I'll install Ubuntu.

Downloaded old ubuntu releases, 4.10, 6.10, 8.10

Finally I have chosen 8.10 and 6.10. What's interesting is gcc already come pre installed, especially the desktop version of the iso.

Try running first few examples, got stuck as there are no core dumped after segmentation fault.

Fixed it by running "ulimit -c unlimited"

Done for today

#### snippets :

- gcc -o program1 program1.c -fno-stack-protector
- ulimit -c unlimited

### 5 April 2020

Understand how stack work, some basic assembly operations (PUSH, POP, CALL, RET, MOV)

Functions and stack - push arguments, call function , then RET stored in EIP

Understand how EBP work, program prologue

Understand basic of 32 bit stack overflow, without stack protection
Disable ASLR just to be safe

Stack boundary and optimisation

If the stack boundary is set to 2, there is no stack optimisation

Overflow a program by inputting a string longer than 30 characters

Debug using gdb and find out where saved EBP and EIP is overwritten in the stack (disable stack protection)

Instead of overwrite with junk, try with real address

Used address of the function return_input

Find the right position of EIP

Program overflowed correctly (printing two times as the EIP call back the return_input function)

#### snippets

- cc -mpreferred-stack-boundary=2 --ggdb program4.c -fno-stack-protector -o overflow
- sudo sysctl kernel.randomize_va_space=0
