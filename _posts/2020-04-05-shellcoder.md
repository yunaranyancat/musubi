---
layout: post
title: "Path to become a shellcoder"
categories: jekyll
permalink : /paths/shellcoder
---

Hey , I'm writing this journal that will be updated periodically at the end of the day as I progress through the Shellcoder Handbook 2nd Edition. I will write anything that I found useful however, do note that this is more of a journal/diary rather than a complete technical writeup, however, once I have mastered the art of shellcoding, I will share some posts. Let's go..

### 5 April 2020

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

### 6 April 2020
