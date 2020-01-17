---
layout: post
title: "VM Aqua Boot2Root Writeup - Speedrun Edition"
categories: jekyll
permalink : /others/vm_aqua
---

Yo! This is my boot2root writeup for **Aqua** vm. For those who didn't manage to play with it yet, download the [vm](#) and come back when you have finished or when you are stuck.

or..., if you want to play with an easier vm, check this [out](https://www.vulnhub.com/entry/nezuko-1,352/).

# About Aqua VM

Name : Aqua

Difficulty : Intermediate to hard

## Enumeration

In this case, the **IP** for the target machine is `10.0.2.6`.

These are the following open ports.

![1](/musubi/assets/aqua/1.png)

When going through the webpage, we found this page.

![2](/musubi/assets/aqua/2.png)

When clicking the **"Sure, I'll help"**
button, we are redirected to another page which shows a potential credential.

![3](/musubi/assets/aqua/3.png)

`megumin:watashiwamegumin`


When running **nikto** on the target we found **login.php**.

![4](/musubi/assets/aqua/4.png)

### Login.php

![4.2](/musubi/assets/aqua/4.2.png)

Using the credential found, we managed to log in.

![5](/musubi/assets/aqua/5.png)


The url is vulnerable to LFI as seen below.

![6](/musubi/assets/aqua/6.png)

## Exploitation

Upon further enumeration, we found that the port 21 can be opened by using port knocking. It was filtered when nmap result showed up. The knockd config file can be found at `/etc/knockd.conf` in the target machine.

![7](/musubi/assets/aqua/7.png)

Image below shows the result before and after port knocking.

![8](/musubi/assets/aqua/8.png)

Using the same credential , we managed to login into the FTP service.

The content of **hello.php** is the same as in the index page of Megumin secret diary we saw last time. This means that if we put our php reverse shell payload in this directory, we can get a shell by browsing through the page using LFI vulnerability found earlier.

![9](/musubi/assets/aqua/9.png)

![9.1](/musubi/assets/aqua/9.1.png)

The directory **"production/"** is writable so we will put our reverse shell in there.

![10](/musubi/assets/aqua/10.png)

The file `notes` revealed the absolute path of the current directory.

![11](/musubi/assets/aqua/11.png)

This means that, by going to `http://10.0.2.6/home.php?showcase=../deployment/production/ourreverseshell.php` , our payload will be executed.

![12](/musubi/assets/aqua/12.png)

## Privilege escalation I

Upon reading `/etc/sudoers` file, we found out that these users can run commands using **sudo** privileges without password.

**Aqua : /root/quotes, /root/esp, /usr/bin/gdb**

**Megumin : /home/aqua/Desktop/backdoor**

Using the same credential, we managed to login as **megumin**.

![13](/musubi/assets/aqua/13.png)

## Privilege escalation II

And as megumin, we can run `/home/aqua/Desktop/backdoor` using sudo privilege.

![14](/musubi/assets/aqua/14.png)

When rerunning nmap on the target, we found that port 1337 is open.

![15](/musubi/assets/aqua/15.png)

We then try to connect to the port using netcat and get a shell.

![16](/musubi/assets/aqua/16.png)

## Privilege escalation III - Easier method

As aqua we can run gdb with sudo privilege without using the password.

![17](/musubi/assets/aqua/17.png)

We can get a root shell using gdb by following command.

`sudo gdb -nx -ex '!sh' -ex quit`

![18](/musubi/assets/aqua/18.png)

## Privilege escalation III - Without using /usr/bin/gdb

For your information, this is my intended path of getting to root shell. But as I want to give a great experience to everyone including those who didn't know about buffer overflow on Linux, so I have decided to make an easier method to get into root.

By running `sudo /root/quotes`, we know that the binary will print out our name and generate a random quote for us.

![21](/musubi/assets/aqua/21.png)

In aqua home directory, we can get the source code for `/root/quotes` and `/root/esp` binaries which is located at this [link](https://github.com/yunaranyancat/personal_projects/tree/master/project_9).

![19](/musubi/assets/aqua/19.png)

We also know that `/root/esp` shows the address of the **ESP** of the machine and that the **ASLR** is not enabled.

![20](/musubi/assets/aqua/20.png)

Based on the source code, the possible vulnerable part is at the **getname** method which uses **strcpy**. If we put a name longer than the size of the buffer, this can corrupt the memory thus can be exploited to gain a shell via buffer overflow vulnerability.

![22](/musubi/assets/aqua/22.png)

By knowing the environment of the target, we will make a debugging machine which is the exact copy of the target OS.

![23](/musubi/assets/aqua/23.png)

It seems like the target OS is using **Linux Lite 3.8 32 bits**.

![lite](/musubi/assets/aqua/lite.png)

To mimic the situation of the target machine, we will download the source code for **quotes.c** and **esp.c** as root then debug it using non root user.

![24](/musubi/assets/aqua/24.png)

By default, **ASLR** is enabled. To disable **ASLR**, run the following command.

`echo 0 | sudo tee /proc/sys/kernel/randomize_va_space`

![25](/musubi/assets/aqua/25.png)

Then compile the binaries using following options.

esp.c : `gcc -fno-stack-protector -z execstack -no-pie esp.c -o esp`

quotes.c ; `gcc -fno-stack-protector -z execstack -no-pie quotes.c -o quotes`

![26](/musubi/assets/aqua/26.png)

Then give sudo privilege to non root user to execute the binary and start debugging.

![27](/musubi/assets/aqua/27.png)

You can use anything you want for the exploit development but in this writeup, I will be using [peda](https://github.com/longld/peda).

Open the binary in gdb by running `sudo gdb -q /root/quotes` .

![28](/musubi/assets/aqua/28.png)

Disassemble the main program using `disas main` .

![29.1](/musubi/assets/aqua/29.1.png)

![29.2](/musubi/assets/aqua/29.2.png)

Disassemble the getname function using `disas getname` and we can see that the method **strcpy** is being called.

![30](/musubi/assets/aqua/30.png)

Let's try to overflow the program by running `r $(python -c 'import sys;sys.stdout.write("A"*100)')` which will print out 100 A's and will be parsed to the program as our **name** variable.

![31](/musubi/assets/aqua/31.png)

It seems like we managed to overwrite the **EIP**. To find the **offset** of the **EIP**, we need to use a pattern of unique strings. Since peda has this functionality, we can use them.

Create a pattern of 100 characters by running `pattern_create 100 pat` . This will store the pattern in a file called **pat**.

![32](/musubi/assets/aqua/32.png)

Rerun the program and parse the **pattern** as the name argument.

![33](/musubi/assets/aqua/33.png)

![33.2](/musubi/assets/aqua/33.2.png)

Using **pattern_search** command in peda. We will find the offset of the EIP which is at **44**.

![34](/musubi/assets/aqua/34.png)

Our exploit should be like this :

**A*44 + [EIP] + padding + shellcode**

Now, to verify if we have the right offset. We need to change our buffer.

`gdb-peda$ r $(python -c 'import sys;sys.Stdout.write(("A"*44) + ("B"*4) + ("\x90"*32) + ("C"*23))')`

![35](/musubi/assets/aqua/35.png)

For padding, we will add **32 bytes of NOPs** (no-operation opcode) so that it will do nothing and keep sliding to the next opcode until it reaches our shellcode. This is normally called as **NOPsleds** or **NOP slides**.

![36](/musubi/assets/aqua/36.png)

As we can see below, after the execution of the **EIP**, our **NOPs** are on top of the stack where **ESP** points to. Based on the disassembled **getname** method earlier, the last instruction is **ret**.

![ret](/musubi/assets/aqua/ret.png)

So if all is good, once **ret** is executed, the opcodes inside the address that is pointed by **EIP** will be executed, which is our **NOPsleds**. So, we need to put the address where our **NOPsleds** is located into our user controlled **EIP**.

So let's put a breakpoint at the **ret** instruction and look at the stack at the moment of the execution.

![37](/musubi/assets/aqua/37.png)

![37.2](/musubi/assets/aqua/37.2.png)

Boom! We hit our first breakpoint!

Now, we can replace our Cs after the padding with the real shellcode. This is the [shellcode](http://shell-storm.org/shellcode/files/shellcode-827.php) that we will be using. You also can use another shellcode which may spawn a reverse shell or anything else.

Rerun the program with modified payload and put a breakpoint at the end of the `getname` method.

![38](/musubi/assets/aqua/38.png)

Once we hit our breakpoint, run `c` to continue the execution.

![39](/musubi/assets/aqua/39.png)

So far so good, now run the binary outside gdb and put in our payload.

![40](/musubi/assets/aqua/40.png)

We managed to get a root shell in our debugging machine.

Now time for the tricky part. The **ESP** of our debugging machine and the target machine is not exactly the same at the moment. This means we need to modify our **EIP** address little by litte until it hits the right place. (It's like playing jackpot, but better.)

### Debugging machine ESP address: 0xbffffbe0 --> Address A
### Debugging machine EIP address: 0xbffff330 --> Address I

### Aqua machine ESP address: 0xbffffc30 --> Address B
### Aqua machine EIP address: ? --> Address II

![41](/musubi/assets/aqua/41.png)

We can see that the **B > A** , means it is possible that **II > I** .We will run our original payload first to see the outcome.

![42](/musubi/assets/aqua/42.png)

We will slowly increment **Address I** by **10h** and wait for the magic to happen.

![43](/musubi/assets/aqua/43.png)

And here we are. We got a shell! And a beautiful ascii art of **Megumin**.

![44](/musubi/assets/aqua/44.png)

Thank you for playing with my machine and do tell [me](https://twitter.com/yunaranyancat) what should I improve on next time. Constructive criticisms are greatly appreciated. But pls don't attack me too much. I'm scared. **>w<**

![bang](/musubi/assets/aqua/bang.gif)
