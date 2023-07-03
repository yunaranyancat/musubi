---
layout: post
title: ""
categories: jekyll
permalink : /notes/kadkahwinapk
---

# Introduction : بِسْمِ اللهِ الرَّحْمٰنِ الرَّحِيْمِ

First and foremost, I would like to express my gratitude to Fareed Fauzi (Ayed) from NetByteSec for providing me with the APK file. Without his assistance, this blog post would not have been possible.

![chat](/musubi/assets/kadkahwinapk/chat.png)

This blog post will be brief and concise, aiming to focus primarily on the malicious aspect of the code within the application.

## pakcik siapa yang kahwin?

![sapa](/musubi/assets/kadkahwinapk/sapakahwin.jpeg)

Recently, there have been reports about scammers attempting to deceive elderly citizens by luring them into installing a malicious application under the pretext of an invitation to a fictitious wedding.

## MainActivity

![decompiled](/musubi/assets/kadkahwinapk/decompiled.png)

When decompiling the application, several classes can be observed. One of these classes is the *MainActivity* class.

![main](/musubi/assets/kadkahwinapk/main.png)

This class serves as the entry point of the Android application and typically loads the *onCreate()* method.

Within the *onCreate()* method, various actions are performed. These include loading a URL from https://ejemputan.com/kadkahwindigital and checking the Android version of the device.

![check1](/musubi/assets/kadkahwinapk/check1.png)

Starting from line 44, the code checks for the permission to read SMS. If the permission has not been granted yet, the *requestPermissions()* method is called. This method essentially prompts the device to grant permission for reading SMS.

## What the bot?

![bot1](/musubi/assets/kadkahwinapk/bot1.png)

Based on the result of the permission check, these methods execute specific actions. Regardless of whether the permission is granted or not, they send an HTTP request to a Telegram bot. The bot then processes the result as a Telegram message using the *sendMessage* API endpoint from Telegram.

The purpose of the bot is to notify the attacker about the status of the permission, indicating whether it has been granted or not.

![bot2](/musubi/assets/kadkahwinapk/bot2.png)

Furthermore, the application also sends device details to the attacker. This could include information such as the device model, operating system version, unique identifiers, and other relevant device-specific details.

![deviceid](/musubi/assets/kadkahwinapk/deviceid.png)

## ReceiveSMS

![receivesms](/musubi/assets/kadkahwinapk/receivesms.png)

This class contains a method to handle incoming SMS messages.

![bot3](/musubi/assets/kadkahwinapk/bot3.png)

The SMS is parsed and sent to the Telegram bot for further processing.

## SendSMS

![sendsms](/musubi/assets/kadkahwinapk/sendsms.png)

The method essentially sends an SMS to the target number and notifies the Telegram bot about this activity.

## exit

It appears that the bot is still operational, so it is crucial to exercise caution and avoid installing any unfamiliar APK files.

![testapi](/musubi/assets/kadkahwinapk/testapi.png)

Thanks for reading! See ya.

![kazuma](/musubi/assets/kadkahwinapk/kazuma.gif)
