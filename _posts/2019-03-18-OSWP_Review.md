---
layout: post
title: "From 0 to OSWP"
categories: jekyll
permalink : /others/oswpreview
---

Heyyy guys, what's up. Few days ago I just received a mail from Offsec saying that I have passed the Offensive Security Wireless Professional exam. So I'm thinking about writing my OSWP journey from start until end.

### About WiFuv3

![WiFu](/musubi/assets/oswp/wifu.jpeg)

Supposed that you have read everything listed in this [page](https://www.offensive-security.com/information-security-training/offensive-security-wireless-attacks/) , WiFu course is aimed for those who wanted to;

* Know in depth about how 802.11 networks work
* Execute attacks related to WEP, WPA/WPA2
* Learn tools like Aircrack-ng, Pyrit, Kismet etc..
* Prepare your own home based lab related to WiFi hacking

### Requirements

You'll need to have some experience in basic Linux commands, understand basic TCP/IP and OSI model.

Other than that, unlike OSCP or other Offsec courses, you'll need to set up your home lab to follow along with the course. The recommended setup is available on the site, but for me, I used:

* D-Link DIR 601 router
* ALFA AWUS036NHA wifi card

### Getting Started

I received my course content on Wednesday, 6 March. Actually I was expecting to receive them earlier, and it seemed like my first purchase attempt was failed. Make sure you do not add any symbol on the phone number. I think maybe it was because I added +33 in front of my phone number. So, I contacted Offsec and made a second purchase. Don't worry, if you accidentally purchased the course more than two times, you can send a message to Offsec for a refund.

![fail](/musubi/assets/oswp/failpurchase.png)

You will be given a BackTrack ISO, PDF and videos. Since it is recommended to use the given ISO, so I opted out with that ISO. Although at first, I encountered some problems setting up my wifi card to work with it. Maybe it was because I was using VirtualBox instead of VMWare.

### Why I choose this course

Firstly, I wanted to learn more about Wireless network and security. I wanted to know what made them insecure and how to protect against different attacks. Secondly, I didn't save up much money , where at first I wanted to do OSCP. Since I saw that this one is cheaper (450USD), and easier than OSCP, then I thought to myself, maybe I can take this first then do OSCP, or maybe I just want to procrastinate. :x

### Let's GO!

I started by reading the PDF contents before jumping to the videos. At first, you will be introduced to some WiFi theories, 802.11 families , etc. It might sound boring and you may feel that you wanted to jump directly to hacking the WiFi. But, trust me, you wouldn't want to skip those theories. Even if it won't help you in the lab later, you won't get the most out of the WiFu course and it's not really worth it to skip these learning.

But if you are a pro master hacker who can hack every WiFi network in this world, so you can jump directly to videos and play with your lab. Hehe

After reading those theories, I directly jumped to watching videos. For me, I only focus on finishing the videos while using the PDF as my reference. So, I use both of them at the same time and it's quite efficient I suppose?  

### Booking For Exam

Once I feel confident about passing the exam, I went to the site and booked for the exam which will be held on March 18. But, when it seemed like I started to procrastinate, I changed the booking date to be earlier, which is on March 14. Then, I spent the remaining days repeating different attacks in my home lab with different security level.

### Exam Day

You will be given 3 hours 54 minutes to complete the exam. There are three different APs with different security level and you need to get the keys using various attacks.

I was able to solve all of the challenges without any problem and then I managed to send the exam report on the same day.

The secret ingredient to pass the exam is `don't forget what you have done in the lab`. :D

### OSWP Certified Hype Day

The next day, I received the email saying that I passed the exam and got certified.

![email](/musubi/assets/oswp/email.png)

Thank you for reading the post and if you have any further question related to OSWP (no spoiler ofc), feel free to send me a message. See you next time. :)
