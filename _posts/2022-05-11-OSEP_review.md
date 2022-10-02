---
layout: post
title: ""
categories: jekyll
permalink : /others/osepreview
---

Hey guys, I'm back!

![kongming](/musubi/assets/osep/kongming.gif)

(and I'll be gone and will be back again.. it will be an endless circle.)

I will be sharing my [OSEP](https://www.offensive-security.com/pen300-osep/) experience (of course, without disclosing confidential information that's only available to OSEP students) and some tips that I have used which helped me in the course and also how to maximize knowledge gained so you can apply it in your future pentest engagement!

Let's go!

# Pre OSEP

Before taking the course, you need to make sure that this course is right for you.

Before I decided to enroll in this course, I already have basic knowledge about Active Directoy infrastructures, configurations, objects, forests, domains, trusts, communications and most importantly what is AD for and why organizations use it especially in larger scale ones.

I also have participated in red team engagements so I have some ideas on how to conduct a successful red teaming assessment.

For me personally, the [syllabus](https://www.offensive-security.com/documentation/PEN300-Syllabus.pdf) provided by Offensive Security suits well as a foundation in performing a red teaming operation.

# OSEP

There will be lots of things you will learn and some of them are:

1. How to gain initial foothold/social engineering (Through phishing, web exploitations, etc..)

2. How to attack AD internal network (Misconfigurations abuse, lateral movement, common AD/system vulnerabilities, SQL servers, etc..)

3. While doing 1. and 2., you will also learn how to bypass standard antivirus programs and application whitelistings so that your payloads/C2 are working as intended.

Below are the methods (although it's rather conventional) I have used to gain the most out of the course :

1. Open the course videos on one screen and the corresponding PDF content on the other.

2. While watching and reading, take note of the code snippets, specific keywords, new terms, so you can perform a deeper research outside of the course.

3. Whenever you're stuck, pause the video, stop scrolling the PDF and read the documentation that's available online.
For example, if you don't know what is **MessageBox**, you can read the official documentation [here](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-messagebox) or for .NET,	[here](https://docs.microsoft.com/en-us/dotnet/api/system.windows.forms.messagebox?view=windowsdesktop-6.0) , you also will be interested to read [this](https://docs.microsoft.com/en-us/dotnet/api/system.windows.messagebox?view=windowsdesktop-6.0). Try to understand what are the differences between each **"MessageBox"**.

4. Do the exercises, extra miles exercises and also the lab exercises. You will learn a lot.

# Post OSEP

I am planning to take **Red Team Ops (CRTO)** from **Zero Point Security**. I've been told that it would be a great add to what PEN-300 currently offer, and based on some reviews, the exams are way harder than OSEP (they said using bloodhound is a no no). So, I hope, with these two courses, I will be able to broaden my knowledge so I can perform a better red teaming operation than before.

![harold](/musubi/assets/osep/harold_meme.jpeg)

### Tips and Tricks

1. Understand different ways to exploit AD environment and which settings/misconfigurations can be abused. Apart from what's in the course, find other vulnerabilities and do take time to understand which component makes it vulnerable.

2. Don't forget about low hanging fruits (sometimes, it is easier/more direct than what you think it is).

3. Don't forget simple enumeration techniques, try to understand in what context are you currently in (are you a local user?,or are you a domain user?, are are you a service account? are you currently in a database server?, or are you in a web server?).
If you are in a webserver, what juicy information can you extract? If you are in a database server, what exploits can you perform? Everything in AD can be treated as objects so it is easier for you to map out it's architecture. (or you can just run [bloodhound](https://github.com/BloodHoundAD/SharpHound) lol)

4. Upgrade your red team arsenals (find more POCs, find more obfuscation tools, craft your own obfuscators, enhance current available obfuscators, craft your own C2, etc etc.. there is no limit to your creativity)

5. Take breaks occasionally. Sometimes, you may think you have reached a dead end / rabbit hole (because your brain is too tired). Keep in mind that there are different ways to achieve the same objectives. The ideas will come eventually when you have calmed yourself.


### Additional references/good reads/study materials

i. [hacktricks AD methodology](https://book.hacktricks.xyz/windows-hardening/active-directory-methodology)

ii. [interesting code snippets](https://github.com/chvancooten/OSEP-Code-Snippets)

iii. [PayloadAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Active%20Directory%20Attack.md)

iv. [example of AD attacks](https://shenaniganslabs.io/2019/01/28/Wagging-the-Dog.html)

v. [c2 obfuscation](https://s3cur3th1ssh1t.github.io/Customizing_C2_Frameworks/)

### Enum tools

i. [winpeas](https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS)

ii. [lse.sh](https://github.com/diego-treitos/linux-smart-enumeration)

iii. [powerview](https://book.hacktricks.xyz/windows-hardening/basic-powershell-for-pentesters/powerview)

iv. [powerupsql](https://github.com/NetSPI/PowerUpSQL)

v. [adPeas](https://github.com/61106960/adPEAS)


p/s : I also have some interesting pocs in my Twitter [likes](https://twitter.com/yunaranyancat/likes) (but... you would need to filter some of the non related stuffs).


# End

Thank you for taking your time reading this review.
If you are planning or currently taking PEN-300, I wish you the best of luck and always remember, try harder!


![gl](/musubi/assets/osep/goodluck.gif)

# Credits

Great thanks to mee-zuh, y4t0 and those who have direct or indirectly contributed in the success of this journey.
