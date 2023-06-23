---
layout: post
title: ""
categories: jekyll
permalink : /notes/kadkahwinapk
---

# Introduction : بِسْمِ اللهِ الرَّحْمٰنِ الرَّحِيْمِ

First and foremost I would like to thank Fareed Fauzi (Ayed) from NetByteSec for providing me the APK file. Without his help, this post would never be made.

![chat](/musubi/assets/kadkahwinapk/chat.png)

This post will be short and straight to the point as I want to focus on the malicious part of the code in the application.

## pakcik siapa yang kahwin?

![sapa](/musubi/assets/kadkahwinapk/sapakahwin.jpeg)

Recently, there are news about scammers trying to trick elderly citizens to install a malicious application by inviting them to a non-existent wedding.

## MainActivity

![decompiled](/musubi/assets/kadkahwinapk/decompiled.png)

When the application is being decompiled, there are few classes that can be seen above. One of it is *MainActivity* class.

![main](/musubi/assets/kadkahwinapk/main.png)

This class serves as the entry point of the Android application, and will normally will load the *onCreate()* method

The method performs some actions such as loading a URL from https://ejemputan.com/kadkahwindigital and checks the Android version for the device.

![check1](/musubi/assets/kadkahwinapk/check1.png)

On line 44 onwards, we can see that it checks for permission to read SMS and if the permission is not yet granted it will perform *requestPermissions()* method which basically request the permission from the device to read SMS.

## What the bot?

![bot1](/musubi/assets/kadkahwinapk/bot1.png)

This methods perform actions based on the result of the permission check.
Whether the permission is granted or not, it will send a HTTP request to a telegram bot which will then process the result as a Telegram message using *sendMessage* API endpoint from Telegram.

The bot serves like a notification to the attacker whether the permission has been granted or not.

![bot2](/musubi/assets/kadkahwinapk/bot2.png)

Additionally, it will send the details of the devices;

![deviceid](/musubi/assets/kadkahwinapk/deviceid.png)

## ReceiveSMS

![receivesms](/musubi/assets/kadkahwinapk/receivesms.png)

This class contains method to handle whenever there is incoming SMS.

![bot3](/musubi/assets/kadkahwinapk/bot3.png)

The SMS is then being parsed to the telegram bot.

## SendSMS

![sendsms](/musubi/assets/kadkahwinapk/sendsms.png)

The method basically send SMS to target number and notify the telegram bot about the activity.

## exit

It seems that the bot is still working at the moment so be careful out there and do not install any unknown APK.

![testapi](/musubi/assets/kadkahwinapk/testapi.png)

Thanks for reading! See ya.

![kazuma](/musubi/assets/kadkahwinapk/kazuma.gif)
