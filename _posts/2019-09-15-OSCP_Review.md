---
layout: post
title: "From script kiddie to advanced script kiddie : OSCP bedtime story"
categories: jekyll
permalink : /others/oscpreview
---

Hi guys, what's up. Few days ago I just received a mail from Offsec saying that I have passed the Offensive Security Certified Professional(OSCP) exam.

Since I have some time to kill, I will share the journey and preparations that I took before I decided to take OSCP exam and also I will share my labs and exam experience. This post will be quite different compared to my OSWP [review](https://yunaranyancat.github.io/musubi/others/oswpreview) as I won't put much details on the course (there are lots of OSCP reviews that explain what is OSCP, what are the course pre requisites, who should take it, etc..) but rather I will explain on how I prepared myself before taking OSCP. Ok, enough for introduction, let's delve deep into a script kiddie's journey to become an OSCP holder.

I will divide different sections where I will call it as levels. These levels will indicate where I was, what were my skill sets at that time and what did I do to go to the next level..

Bear with me, it's not a short journey..

![hacker_a_day](/musubi/assets/oscp/hacker.jpeg)

### Level 1 (I know Linux but I don't know anything / know a little bit about penetration testing) [Time spent : 1~2 months]

Hello there, I know we are all have been in this place. This is the moment right after we have decided to dive deep into the world of penetration testing, or I would say, ethical hacking.

This is the moment where we got frustrated whenever we fail to understand anything.. the moment where, whenever we wanted to install some distros or some hacking tools but failed, we just paste the error in the forum or a facebook group and ask the pros, "hi there, can anyone help me?", in hope of someone will help and maybe tell us something like, "why don't you read the error from the terminal and then try to search what the error says online?" ; but instead we got told "try harder", "google is your friend" or even a sad facebook emoji reaction with no replies. It's okay. It's totally fine. It's not that they don't want to help you.

Maybe it is another way of teaching you and make you think and realize that, "Let's imagine that you are currently taking OSCP exam and you don't know what to do.. do you ask directly for someone else help? No, you won't, and you can't."

You need to find your way in and out, you need to find an entry point, you need to google every terms that you know and that is when you will understand what do they mean by **google is your friend**.

![desperation](/musubi/assets/oscp/desperation.jpeg)

When I was at this level, I bought a book titled **Penetration Testing: A Hands-On Introduction to Hacking** by Georgia Weidman.

After that, I started playing [HackTheBox](https://www.hackthebox.eu/), which my first machine was [Jerry](https://www.youtube.com/watch?v=PJeBIey8gc4) (Jerry has just got retired when I wanted to try it, I think around early December). At that time, I was pretty happy to get a shell eventhough I was just replicating everything what [Ippsec](https://www.youtube.com/channel/UCa6eh7gCkpPo5XXUDfygQQA) did. (⁄ ⁄>⁄ ▽ ⁄<⁄ ⁄)

### Level 2 (I know what is pentest but I still need to make myself comfortable with hacking or boot-to-root CTF style) [Time spent : As long as it should take]

This is where I spent most of my time playing HTB, met a lot of new friends, shared and exchanged lots of knowledge, joined lots of groups.. and after a month playing HTB, I managed to obtain a **Guru** rank from **script kiddie** rank (I was really close to **Omniscient** as I managed to root all of the active machines at that time but I was stuck at binary exploitation challenges, because, you know... which script kiddie can do binary exploitation? **(>_<)** ) ..

Other than that, sometimes I spent my time playing with other CTF sites such as root-me, rankkk, and other CTF sites but, HTB is number one in my heart. ☆⌒(ゝ。∂)

Check out my [wechall](https://www.wechall.net/profile/y4t0) profile for more info.

### Level 3 (I think I know how to root a machine but now I need to focus more on OSCP style machines) [Time spent : 1 month]

I know I know, it's the infamous OSCP like HTB machines list created by [TJnull](https://twitter.com/TJ_Null).. Hehehe.. So, at this point, I started to do one by one based on that list.

![oscplike](/musubi/assets/oscp/oscplike.jpg)

Okay, these are my tips and tricks on how to gain the most of knowledge for every box that is on the list. (Not just for OSCP)

First, I will enumerate the machine by myself. What do I mean by that is I will try to find running services and then I enumerate the service that I know at that time and when I'm stuck, or when I run out of ideas, only then would I watch Ippsec videos but I will **ONLY** watch the PART where I was stuck.

After that,  I paused the video then proceed my enumeration and then repeat the methods until I manage to root the machine. This will give me new enumeration and exploitation ideas or techniques which I can try to use them on other boxes later on.

After I have rooted a machine, I re-watched Ippsec videos without pausing. This is the moment where I wanted to know how he did it and what's different than what I did and how can I improve my enumeration techniques. The special thing about his videos are he didn't root it only one way, but he also will try to find another way of enumerating the machine. (This also can be done by reading different writeups available online).

And the last cycle is optional, re-root the machine without any help. (Practice what you just learned + what you already know)

I can say that I'm pretty good with Linux machines, so I spent more time on Windows boxes. I learned about Windows environment, such as file transfers, system information, services and others that might give me the idea on how to find vulnerabilities in Windows machine.

### Level 4 (I'm pretty confident to take OSCP exam) [Time spent : 1 month]

I then booked the labs for 30 days, then scheduled the exam a week before my lab time finished (I'm scared that if I waited too long, I may end up procrastinating). Since I was pretty comfortable with my enumeration (to a comfortable level), I didn't play much in the labs. I only used the labs to practice my buffer overflow techniques.

At first, I have decided to focus only on the buffer overflow part on the last week before the exam started, however, when I realized that the buffer overflow part was pretty easy to replicate, it boosted my confidence and that is when I decided to rescheduled my exam to be another week earlier than scheduled, (rescheduled two days before the exam date).

### Level ??? (OSCP exam) [Time spent : 24 hours++]

Targets :
Few machines with one for Buffer Overflow.. (70 is the passing points)

The exam started at 11PM (Normally I'm already sound asleep at this time). I prepared three cans of Redbull besides me just in case my eyes won't open when I wanted it to.. :3

So, my strategy was like this :

- Tackle buffer overflow machine first as it is the easiest one
- While doing Bof, run [nmapautomator](https://github.com/21y4d/nmapAutomator) for the other machines and come back later to run a more thorough manual scans. (nikto, gobuster, nmap scripts scan etc..)

It took me around two hours to complete the buffer overflow part. I did it slow and steady so I that I will know where is the error just in case the exploit didn't work. Once I have finished the buffer overflow part, I focused on my scan results for other machines.

My standard enumeration + exploitation techniques would be ;

1. Identify open ports and what are the running services.
2. Find the version of the running services and compare it to the latest version available online. If the version is quite old, then there's a possibility that it is an entry point into the target.
3. Read the exploit that are related to the running services and understand how it can be exploited.
4. Repeat step 2-3 until you have identified some ports that could be your entry point (some low priv shell or even a root shell ) with different list of possible vulnerabilities.
5. Rabbit holes : IMHO, rabbit holes only exist if you ;
  - Do not understand how the exploit works (If you know how it works but when you run it then nothing happened, then move on to different services/ports, that service might not be vulnerable even though it's version is vulnerable.. we can assume that it has been patched)
  - Keep running the exploit again and again without modifying the exploit code hoping that it will work (Insanity is about doing the same thing over and over again and expecting different results.. The key is to understand what and why it's not working. )
  - Panic and suddenly forgetting every simple enumeration technique that you have learned. (Keep calm and go through your enumeration slowly, one by one. No one will ever know what's waiting ahead, so keep enumerating.. )
6. If all of the above didn't work, then check back :
  - Your scan results (Have you scanned all ports? TCP or UDP? )
  - Did you forget something / enumerating each port that is open? If you are paranoid like me , you can use this [list](http://www.0daysecurity.com/penetration-testing/enumeration.html)
  - Did you forget to modify the script to fit your target environment? (ports? protocols? path? version? command? or even running Operating System?)
  - Did you forget to take A REST? A quick nap or even few minutes of break could help you get back in the game.
7. Profit

Extra note :

__Metasploit usage__

Some of you might find that using Metasploit would be rewarding, however that wasn't my case.

12 hours passed and I have rooted all of the machines except for one machine, which was the 25 points and I only got a normal user shell (low privilege). Since I was very frustrated that my exploits didn't work + I'm so tired and sleepy (I only slept for 3 hours in total during my exam day, 1 hour during the midnight, 1 hour during the early morning and another 1 hour in the evening), I went into ~~rage~~ script kiddie mode.

![noob](/musubi/assets/oscp/noob.jpeg)

I upgraded my shell into a Meterpreter session hoping that **local exploit suggester** in Metasploit would work. However, I found nothing. Nothing at all. Not even a tiny bit of clue popped up after I used my supposedly Metasploit "allowance" . Then I realized that I forgot to do something, one of the most important things to do for everyone especially for those who are taking the exam; **I FORGOT TO TAKE A REST**.

![meterpreter](/musubi/assets/oscp/meterpreter.jpeg)

Then, I went out for lunch with my friends and then I started to calm down. After that, I enumerated the machine slowly and carefully using my low priv shell, and then, I found an entry point. It was just right in front of my eyes. I didn't know how could I have missed it. Then, after few modifications to the exploit code, I managed to get a root shell! Wohooo.. 100 points my boi..

![chika](/musubi/assets/oscp/chikadance.gif)

### Report

Hah! You thought that the suffering has ended don't ya? Yeah, it actually has ended. Now, for most of us, it's a calmer moment. I used this report [template](https://github.com/whoisflynn/OSCP-Exam-Report-Template) then submitted my report on the next day early morning.

### Fin

Few days later, I got a mail telling that I have passed the OSCP exam. Another good news, I slept very well that night.

![chika](/musubi/assets/oscp/sleep.gif)

## TL;DR;I hate weebs;

**Pre-OSCP**

a. Penetration Testing: A Hands-On Introduction to Hacking (Georgia Weidman)

![gw](/musubi/assets/oscp/gw.jpg)
b. TJnull HackTheBox OSCP like machines list


![oscplike2](/musubi/assets/oscp/oscplike.jpg)

**OSCP**

- [nmapautomator]()
- 0 day security [checklist](http://www.0daysecurity.com/penetration-testing/enumeration.html)
- [secwiki](https://github.com/SecWiki/windows-kernel-exploits) Windows kernel exploits
- g0tmi1k linux privilege [escalation](https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/)
- sushant747 windows privilege [escalation](https://sushant747.gitbooks.io/total-oscp-guide/privilege_escalation_windows.html)
