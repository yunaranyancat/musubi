---
layout: post
title: ""
categories: jekyll
permalink : /others/ctfjembalang
---

This is a writeup regarding a CTF challenge for the Battle of 1337 CTF hosted by Tenang Komuniti Sdn. Bhd and Yayasan Digital Malaysia.

![CTF1337](/musubi/assets/jembalang/ctf.png)

You can download the source code [here](https://github.com/yunaranyancat/ctf-dump/blob/main/jembalang.zip).

First, you will be greeted by this locked page.

![lockedpage](/musubi/assets/jembalang/unlockpage.png)

By inspecting the element, it shows a text and an ASCII art of the infamous Mr Trollface.

![trollface](/musubi/assets/jembalang/troll.png)

The password to unlock the page is obvious : **DOWN HERE**

When the page is unlocked, this page will be shown.

![intro](/musubi/assets/jembalang/intro.png)

Disable the jumpscare by unticking the events on the secret field so you will be able to submit the secret.

![disablejumpscare](/musubi/assets/jembalang/nojumpscare.png)

For those who are curious, this is the jumpscare;

![jumpscare](/musubi/assets/jembalang/jumpscare.png)

By going to the debugger, there's an interesting js file, filled with Mr Trollface ASCII art, again.

![dbugger](/musubi/assets/jembalang/debugger.png)

On line 88, you will find a long string of obfuscated javascript code.

![line88](/musubi/assets/jembalang/line88.png)

Decode it using [de4js](https://lelinhtinh.github.io/de4js/) with paramater set to eval.
Here you will see that to unlock the page, you will need to concatenate all of the character taken from the string c.

![de4jseval](/musubi/assets/jembalang/de4js.png)

I'll let you sort it by yourself. ;)

![free](/musubi/assets/jembalang/free.png)

And the end.

![end](/musubi/assets/jembalang/end.png)

it's not that troll-ish , isn't it? (>‿◠)✌
