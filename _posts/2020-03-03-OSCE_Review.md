---
layout: post
title: "Push CTP , Pop OSCE : From advanced script kiddie to expert script kiddie"
categories: jekyll
permalink : /others/oscereview
---

Yee haw! Last week I just received a mail from Offsec saying that I have passed the Offensive Security Certified Expert (OSCE) exam and here is my review.

Right after OSCP, I've been thinking, what should I learn next? So I said to myself, maybe I should try learning basic exploit development..?

![nasahacked](/musubi/assets/osce/nasahacked.gif)

# Pre OSCE

So, I started to learn assembly. I used the materials from this [site](http://opensecuritytraining.info/IntroX86.html) and revised for few weeks. I started to understand what are registers and their purposes, opcodes, how to use debugger, how to compile codes and other basic stuffs.

![compiler](/musubi/assets/osce/compiler.jpg)

Then, I filled some basic knowledge gaps by watching **x86 Assembly and Shellcoding on Linux** videos from **Pentester Academy**.

![pentestacademy](/musubi/assets/osce/pentesteracademy.png)

Okay, from here, I went to Corelan [site](http://opensecuritytraining.info/IntroX86.html) to improve my binary exploitation skills. These are the topics that I have covered for my preparation before enrolling in CTP.

- Exploit writing tutorial part 9 : Introduction to Win32 shellcoding

- Exploit writing tutorial part 8 : Win32 Egg Hunting

- Exploit writing tutorial part 7 : Unicode – from 0x00410041 to calc

- Exploit writing tutorial part 3b : SEH Based Exploits – just another example

- Exploit writing tutorial part 3 : SEH Based Exploits

- Exploit writing tutorial part 2 : Stack Based Overflows – jumping to shellcode

- Exploit writing tutorial part 1 : Stack Based Overflows

I also went to [fuzzysecurity](https://www.fuzzysecurity.com/tutorials.html) to improve my skills. If you don't know where to start/stop, just follow and understand what are taught below.

![fuzzysecurity](/musubi/assets/osce/fuzzysecurity.png)

When I thought that I'm pretty comfortable with basic exploit development, I started to enroll in the CTP course. But, wait! I need to solve the pre registration challenges [first](http://fc4.me/).

![fc4](/musubi/assets/osce/fc4.png)

At first try, they were pretty hard, so I thought maybe, just maybe... because I procrastinated for few weeks before, I almost forgot what I have learned... Dang it!

So, I went back to reread all of the courses/topics mentioned above, then the second time I went to the site to complete the challenges, I passed and managed to get the registration key. The key was "tryharderlolimjokingthisisnotthekey" .

# OSCE

So, my journey has started. I used these learning techniques to improve my understanding of the syllabus in the course;

- watch the videos and follow along to complete the exercises
- repeat until you don't need to watch the videos to complete the exercises

![hacc](/musubi/assets/osce/hacc.jpg)

The method is quite repetitive and for lazy ass people like me, I tend to feel bored really fast and started to procrastinate. What I did to kill the boredom was, for topics like binary exploitations, AV bypass, PE backdooring, I looked up different PE online and then I applied what is taught in the course.

Of course, the practical applications will be quite different but the theories on how to exploit the binaries still remain the same. Sometimes, I encountered some problems, and with these problems, I managed to understand more and more about binary exploitations. So, basically, this is one of the ways to learn things, learn from mistakes.

Well the drawbacks are, you will start to overthink and sometimes will forget the basic but important things/theories. So, to overthink or not to overthink, it's your choice.   

Then comes the exam time. So, in the exam, you will be given some amount of challenges and you will need to solve it within 48 hours and you will have 24 hours to do the report. Stay calm and don't panic. Take a rest. I REPEAT. TAKE A REST IF YOU ARE EXHAUSTED.

![busy](/musubi/assets/osce/busy.jpeg)

# Post OSCE

So... there is this one book that I bought as a prep to enhance my binary exploitation skills which is **The Shellcoder Handbook (2nd Edition)**. After reading few pages, I realised that, this book is way bit more advanced than what I expected, so I put the book aside first. :>

After I have passed OSCE certification exam, I think now it's time to finish that book and level up!

![doge](/musubi/assets/osce/dogehacker.jpg)

### Tips and tricks

So, here are the tips and tricks that I wanted to share with you guys;

1. Watch all of the videos and red everything in the PDF. Do not focus only on the binary exploitation part.
2. Do all of the exercises again and again and again and again and again. (Trust me, you'll thank me later for this.)
3. Do not focus only on what is taught in the course, try to go a little bit further and explore as much as you want, but don't dive too deep, you'll drown yourself.
4. Some people will say that everything you need to pass the exam is in the course, yes, but.. no.. It's like, if **A** is in the course, maybe **a** or **@** or even **∀** is in the exam. So, it's not **A** for **A**. (I don't really know what I'm talking about lel)
5. Don't rely too much on pre generated shellcode, try to create your own shellcode from scratch. [Check this out](https://www.exploit-db.com/docs/english/17065-manual-shellcode.pdf).

These are some OSCE reviews that I found helpful on providing useful resources :

i. [https://www.thesubtlety.com/post/2017-02-11-osce-review/](https://www.thesubtlety.com/post/2017-02-11-osce-review/)

ii. [https://tulpa-security.com/2017/07/18/288/](https://tulpa-security.com/2017/07/18/288/)

iii. [https://aminbohio.com/study-guide-tips-offensive-security-certified-expert-osce-cracking-the-perimeter-ctp/](https://aminbohio.com/study-guide-tips-offensive-security-certified-expert-osce-cracking-the-perimeter-ctp/)

### FAQ

**Q1**: Hi yunaranyancat, the course is outdated, should I take it? Or should I wait until they update the course?  
**Meow**: Hi random stranger, for me personally, there are quite amount of difference in knowledge before and after I enrolled in CTP. For me, I agree the techniques might be outdated, but what I think is, Offsec is trying to teach you the foundation of the methodologies focused on exploit development, advanced web attacks, WAN attacks , etc.. Of course they can update it to fit the latest trends of AV bypass techniques, advanced binary exploitation, hacking SCADA or anything, but IMHO, these should be in the OSEE domain. Like OSCP, what they are trying to deliver is the foundation of the pentesting methodologies, and it is up to us to further enhance it and try to keep up to fit in current trends. However, I won't deny that the techniques are outdated but the methodologies that I gained can be applied on today's exploitation. I'm not saying I'm an expert so I'll just say I'm the pro in the script kiddies domain.

**Q2**: Is taking OSCE the only way I can learn binary exploitations/exploit development?  
**Meow** : No, not at all. There are other courses that offer the same thing such as eLearnSecurity XDS , Corelan trainings, etc.. You don't even need a cert to say that you "know" exploit development. For me, certifications act as my own way of benchmarking my skillset. I know that I will surely forget what I have learned in OSCE, OSCP etc.. if I didn't maintain my technical skills, so it is up to my responsibilities to maintain my qualifications as an OSCE holder. You don't need to do the same way if you don't want to. Knowledge can be gained even without certifications.

**Q3**: Hi, can you tell me what are the exam objectives?  
**Meow** : Meow!

# Fin

Thank you for reading! See you next time.

![shaw](/musubi/assets/osce/shaw.gif)
