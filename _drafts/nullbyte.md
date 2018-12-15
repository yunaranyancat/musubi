---
layout: post
title: "Write-up for Nullbyte"
categories: jekyll
permalink : write-ups/nullbyte
---

#### Nullbyte 0x00 level 1

For today, I will show you how I gained root on [Nullbyte : 1](https://www.vulnhub.com/entry/nullbyte-1,126/).

First, I ran ifconfig and netdiscover to find my Kali ip and the target ip.
>ifconfig
>netdiscover -i eth0 -r 192.168.1.0/24

![ifconfig](/assets/nullbyte/ifconfig.png)

Based on netdiscover, I found out that the target ip is 192.168.1.142

![netdisc](/assets/nullbyte/netdiscover.png)

Nmap scan revealed that there are three open ports...

![nmapscan](/assets/nullbyte/nmap_scan.png)

So, I decided to go for the first port, I opened the browser and browsed to the target server.

![eye](/assets/nullbyte/website.png)

> " If you search for the laws of harmony, you will find knowledge "

Since this is a CTF-like box, I thought there might be a clue hidden between those words or even the image!

So I saved the image and run few commands that are usually use for steganography.

first, I ran;
> strings main.gif

to find if there are any hidden text, and sadly I didn't find anything.. although there was something unusual printed on the second line, I might keep an eye for it.

![strings](/assets/nullbyte/strings.png)

using other tool such as Magick, I found something that is exactly as same as what's printed by the strings command.

![magick](/assets/nullbyte/magick.png)

Seriously , I wasn't very sure what should I do with the message, so I tried to find something hidden on the page using normal Ctrl+i (inspection), but nothing there, then I tried to paste it after the ip on the url, then this happened;

![secretdir](/assets/nullbyte/secret_dir.png)

The first thing that came to my mind was to try whether the form is vulnerable to SQLi or not, but based on the inspection of the page, the form wasn't connected to the database;

![nosqli](/assets/nullbyte/nosqli.png)

Since the clue said that the password isn't that complex, let's go with brute force/dicitonary attack.

I needed the error so I can pass it to hydra.

![invalidkey](/assets/nullbyte/invalidkey.png)

I ran;

> hydra -l "" -P /usr/share/wordlists/rockyou.txt -v 192.168.1.142 http-form-post '/kz.../index.php:username=^USER^&key=^PASS^:invalid key'

![hdyra](/assets/nullbyte/hydra.png)

After few moments, hydra cracked it and the password was **elite**

![hdyra_elite](/assets/nullbyte/elite_hydra.png)

Then, I was redirected to another page, with another form and another job to do... (⁎˃ᆺ˂)

![elite](/assets/nullbyte/elite.png)

I went back to the idea where I wanted to try whether the form is vulnerable to SQLi or not, so I added the symbol `"`

![hdyra_elite](/assets/nullbyte/vulnerable.png)

So, based on the image above, the SQLi might work, so I put `" or ""="` , and voila!
For your information, `" or ""="` will always return true on the database query so it will show the query result of the database..

![sqli](/assets/nullbyte/sqli_ed.png)

Based on the image above, we found out there are two users, `ramses` and `isis`.

so, let's bruteforce the ssh login using the first user, `ramses`.

> hydra -l "ramses" -P /usr/share/wordlists/rockyou.txt -V -s 777 192.168.1.142 -I -t 4 ssh

this command basically tells hydra to use dictionary attack to get into port 777 with username `ramses`.

![bruteforce](/assets/nullbyte/bruteforce_hydra.png)

and then after a while, we got the pass!

![omega](/assets/nullbyte/omega.png)

So, we will log in via ssh using the creds we got. (ramses:omega)

once I logged in, I did some simple enumeration and found something interesting in ramses `.bash_history` file.

![ssh](/assets/nullbyte/ssh.png)

Then , I went to the place where `procwatch` is located. I found out that `procwatch` has a SUID byte and maybe this can be exploited.

useful link : https://www.linuxnix.com/suid-set-suid-linuxunix/

So, I tried to run `procwatch` and the output showed something similar to when we run a normal `ps` command. So, I assumed that the command will just run the `ps` command without any other modification.

![procwatch](/assets/nullbyte/procwatch.png)

So, for this exploit, we need to trick the machine to run our `ps` instead of running linux `ps` command. In order to do this, we need to create a `symlink` that will spawn a shell instead of showing processes when we call the `ps` command.

> ln -s /bin/sh ps

Then , we need to update our $PATH variable so that when procwatch is being run, it will call the ps in our current directory (the **fake** one that will lead to spawning a shell) instead of calling the ps in /bin. So, we need to do this

> export PATH=.:$PATH

in our current directory. And then , try to run `./procwatch` to see if it works.

![rooted](/assets/nullbyte/rooted.png)

And we got root!

Thank you, that's all for today.
