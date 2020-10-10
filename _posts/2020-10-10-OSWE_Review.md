---
layout: post
title: ""
categories: jekyll
permalink : /others/oswereview
---

Good day everyone! It has been a long time since I updated my site. I was pretty busy with work and stuff so I think I've been idling for a quite amount of months now. But here I am, writing a post to share to you guys some very helpful tips and tricks for **AWAE**. I'll also share some good AWAE reviews that I find very helpful.

Just for a side story, I got a technical interview for Vulnerable Machine Engineer at Offensive Security just two days before my exam and sadly, I didn't nail it (but don't worry, I know you guys will, keep the applications coming in), feelsbadman. Nah it's okay, I'll try again in the future.

P/S : For those who are interested to try my vulnerable machines, they are available at Vulnhub, [Nezuko](https://www.vulnhub.com/entry/nezuko-1,352/) and [Aqua](https://www.vulnhub.com/entry/aqua-1,419/). I keep telling my friends that there will be a third machine incoming, but I still can't find a suitable time to do it, cause, you know, busy with life and work.

![holiday](/musubi/assets/oswe/holidays.jpeg)

Hmm, how about a whitebox approach for my next box? >w<

# startx

So first, what is [AWAE](https://www.offensive-security.com/awae-oswe/) all about?

**Advanced Web Application Exploitation** or in short, **AWAE** is a course that is more focused on the whitebox approach of penetration testing. The "how the vulnerability works"  as in the code level of the application.

For me, this is very interesting, because you will get a good understanding on "why this part of application flow is vulnerable" and you'll eventually be able to see how to exploit that particular code / functions in an efficient way.

Other than that, since you got all the codes to yourself , hehe , you need to take it to the next level where you need to chain multiple vulnerabilities to create an exploit that is particularly important / CVE-worthy, I guess?.

![allgetcve](/musubi/assets/oswe/allgetcve.jpg)

# Pre OSWE

Before taking the course, you need to decide whether this is the right course for you or not. For me, working as a full time penetration tester, I know this is a good addition to my current skill set. I also have done source code reviews before so I wanted to know Offsec's approach on a whitebox environment and try to improve my code review ability.

However, this course is more focused on **web application** part of the **code review**, so bear in mind, web code review is only a small part of the overall source code review. You will need to familiarize yourself with programming languages such as PHP, JAVA, .NET, .. etc. You can check their [syllabus](https://www.offensive-security.com/documentation/awae-syllabus.pdf) if you don't know which languages you should familiarize with for the course.

In addition, I would suggest on creating a simple web app using these languages so you'll get to know what are the common libraries used, functions, implementation, how to debug, how to perform [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) on the application, and basically, becoming a "junior" web app programmer. Hahaha.

![junior](/musubi/assets/oswe/junior.jpg)

To know how the code works, they will give you an advantage on where to find the vulnerable parts. - cited from **Top 10 Yunaranyancat Quotes (2020)**

# OSWE

These are the methods on how I proceeded with the course;
1. Watch the videos and try to replicate what the instructor did in the provided lab. Don't waste your time if you have only 30 days of lab time.
**Watch, Pause, Replicate, Resume.**
Keep in mind, you can replay the videos and reread the PDF multiple time, but the lab time is limited. However, if you have a lot of money, which is not in my case, you can purchase additional lab time. (free advertising there, I see)

2. For each chapter completed, pause the videos, open the PDF provided to see if you are missing something. As far as I know, the PDF course is more exhaustive than the videos. The videos are only for you to reach your objectives while the PDF is to explain to you how the objectives are being achieved , step by step. However, if you ever feel that for some part of the course, the explanation is still not enough, well, that is what Google for!
![crime](/musubi/assets/oswe/crime.jpg)

3. Once you have completed the PDF, videos, labs and the extra miles exercises, what you need now, is a good methodology to do web application code review efficiently.

### Tips and tricks

Here are some tips and tricks that might be useful for you guys;

1. Understand how web application works. For example, the routing of the web, the input validation, CRUD process, function definitions. As one of my friends said, most of the vulnerabilities come from insufficient input sanitization!
2. Try to familiarize yourself with common vulnerable functions, such as **exec()**, **system()**, **eval()**, serialization and deserialization functions, weak regex, insufficient input sanitization, until the moment when you see a code, you are pretty confident to decide whether it's vulnerable or not, after that, it's up to you to test it with different payloads or try to bypass any restriction.
3. Since it's a whitebox approach, you'll have an access to the debugging environment. So, for example, if you want to check if your SQL injection payload is working correctly or not, you can run the statement in the SQL environment of the target. Or, if you want to see what function is being called when you send or receive a request, you can set up the breakpoints using debugger on the running code.
4. Sometimes, it's hard for some people to find the right portion of the code, so the key here is, try to understand what are the objectives that you are trying to achieve, and see which part of the code that is related to the objectives, well unless you say, everything is related, I don't know what to say.
![pepe](/musubi/assets/oswe/pepe.png) <br>
What I mean by **"something that is related"**, it's like, **"when you do this, this part of the code will be executed."**

5. Don't forget to take a break. Sometimes, looking too long at the code will only give you stress and limit your thinking capabilities (like what I experienced). So, the best way is to take a short break anytime you need it.

# Post OSWE

I might take a look on incoming Offensive Security courses, but I will read the reviews first to see whether they're interesting/worth it or not.

[Check them out!](https://www.offensive-security.com/offsec/retiring-ctp-intro-new-courses/)


### Some good AWAE reviews/tips/tricks

These are some OSWE reviews that I found very helpful:

i. [https://forum.hackthebox.eu/discussion/2646/oswe-exam-review-2020-notes-gifts-inside](https://forum.hackthebox.eu/discussion/2646/oswe-exam-review-2020-notes-gifts-inside)

ii. [https://github.com/wetw0rk/AWAE-PREP](https://github.com/wetw0rk/AWAE-PREP)

iii. [https://github.com/timip/OSWE](https://github.com/timip/OSWE)

# End

Thank you for taking your time reading this post! ,and sorry for the memes too if it's annoying. Ciao.

![paimon](/musubi/assets/oswe/paimon.gif)

## Credits

Special thanks to those who have helped me throughout the journey. Without you guys, I won't be able to reach this far... You guys rocks! And also to my lovely fiancée, thanks for being with me through thick and thin! I love you! uWu

![lolihug](/musubi/assets/oswe/lolihug.gif)
