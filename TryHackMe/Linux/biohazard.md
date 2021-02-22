# Biohazard - Medium

![biohazard](https://tryhackme-images.s3.amazonaws.com/room-icons/72aca9d285c3156a05b34b7f6cc67ae6.png) 

## Solving:

### Task 1:

First we run our standard NMAP scan ```sudo nmap -sC -sV -p- -oA scan 10.10.5.66 -vv```
Key info:

```bash
PORT   STATE SERVICE REASON         VERSION
21/tcp open  ftp     syn-ack ttl 63 vsftpd 3.0.3
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 c9:03:aa:aa:ea:a9:f1:f4:09:79:c0:47:41:16:f1:9b (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDM1/tmq8Lrur25evbyyI7/+nxDlhbVbMMiRfz5a0eI7Sq9yODJGCVNMPJGKOwtgA/BlPi7V3TKyYJVeH1QOzP8mPLVgfYom6ovelJiLiR6VrO4dqxx+G3ir+tj/OOSc4MpmdnqCvQKtAeJ4e5bbWakFihXyy14yi++oOzqp2VDlqMNN+d2k0uSAx1rDbngwP3UvRfE1E1TaSYhljnb9kvWRxBABhpdkUjbcRLwxBAQFBm9Vm+yQYPurC9YJ1BUlJzOFesYnbS27bG1vVCcuPQN3YjcljVCXBdd0qIvZdYlez4+mVUcJJh1iWl83sfgo+wZRmfHsedjdL1eWNrkt+ed
|   256 2e:1d:83:11:65:03:b4:78:e9:6d:94:d1:3b:db:f4:d6 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBNy83txF27peDYxMhrPqfipXwZtBNY9H4fww7f2FRCkt09tEcp5f5BKhOE4cNo033XYpmaowy1r4qgFpIqKjf64=
|   256 91:3d:e4:4f:ab:aa:e2:9e:44:af:d3:57:86:70:bc:39 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMhTmk6F06eyLfM0j07nUcnqMqGdgOfFqsp3eLdbwwn0
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.29 ((Ubuntu))
| http-methods: 
|_  Supported Methods: POST OPTIONS HEAD GET
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Beginning of the end
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel


```

We can go to port 80 and have a look.
![](https://i.imgur.com/U4E7tfq.png)

I downloaded the image just incase there's any steganography going on. It appears it's password protected so may look in to this later.

I go through the page and click the link in to /mansionmain/. I also run a Gobuster on the website root directory.
```bash
$ gobuster dir -u http://10.10.5.66/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -e -x html,php,txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.5.66/
[+] Threads:        10
[+] Wordlist:       /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Extensions:     php,txt,html
[+] Expanded:       true
[+] Timeout:        10s
===============================================================
2021/02/22 15:45:47 Starting gobuster
===============================================================
http://10.10.5.66/images (Status: 301)
http://10.10.5.66/index.html (Status: 200)
http://10.10.5.66/css (Status: 301)
http://10.10.5.66/js (Status: 301)
http://10.10.5.66/attic (Status: 301)
http://10.10.5.66/server-status (Status: 403)
===============================================================

```

![](https://i.imgur.com/WwXb497.png)

Another image to download and look at. A quick look at the source shows the comment leading us to the next directory.

```html
<!-- It is in the /diningRoom/ -->
```

![](https://i.imgur.com/LImCAjo.png)

Again, download the image to check and look at the source. We see a html comment that looks like it's base64 encoded.
```html
<!-- SG93IGFib3V0IHRoZSAvdGVhUm9vbS8= -->
```

```bash
$ echo "SG93IGFib3V0IHRoZSAvdGVhUm9vbS8=" | base64 -d
How about the /teaRoom/
```

We also pick up the emblem flag by navigating to an emblem.php document.

<details>
 <summary>emblem.php</summary>
emblem{********************************}

Look like you can put something on the emblem slot, refresh /diningRoom/
</details>

We refresh /diningroom/ and input the flag in the field. It redirects us to /diningRoom/emblem_slot.php with the text 'Nothing happen'.
I'll ignore this for now and continue with the /teaRoom/ suggestion.

![](https://i.imgur.com/YN45wTx.png)

We get the lockpick flag and then move on to /artRoom/

<details>
 <summary>lockpick flag</summary>
/teaRoom/master_of_unlock.html
lock_pick{*******************************}
</details>

![](https://i.imgur.com/FoQ0oKe.png)

Here we see a link to a map. 

<details>
 <summary>Map</summary>
http://10.10.5.66/artRoom/MansionMap.html

Look like a map

Location:
/diningRoom/
/teaRoom/
/artRoom/
/barRoom/
/diningRoom2F/
/tigerStatusRoom/
/galleryRoom/
/studyRoom/
/armorRoom/
/attic/

</details>

I try the next directory on the list. /barRoom/

![](https://i.imgur.com/0ydTZNJ.png)

We input the lockpick flag and get to another page.

![](https://i.imgur.com/Bk7skb5.png)
Well we don't know the piano or sheetmusic flag yet so we have a look at the link.

>Look like a music note
>NV2XG2LDL5ZWQZ**************************WGNSGCZLDMU3GCMLGGY3TMZL5 

Looks a bit like Base32, and sure enough (using cyberchef)

<details>
 <summary>music_sheet flag</summary>
music_sheet{***********************************}
</details>

We put the flag in to the input box to be redirected to the Secret bar room.

![](https://i.imgur.com/WaunxhO.png)

Click yes to get to the gold_emblem flag.

<details>
 <summary>gold_emblem page</summary>
http://10.10.5.66/barRoom357162e3db904857963e6e0b64b96ba7/gold_emblem.php

d_emblem{*******************************}

Look like you can put something on the emblem slot, refresh the previous page

</details>

![](https://i.imgur.com/LphSJcy.png)

I put in our first emblem flag and got back the text ```Rebecca``` We can save this for later.

I went down the map list to the next directory. /diningRoom2F/ 

![](https://i.imgur.com/ZXmyeSO.png)

In the source there is a html comment 
```html
<!-- Lbh trg gur oyhr trz ol chfuvat gur fgnghf gb gur ybjre sybbe. Gur trz vf ba gur qvavatEbbz svefg sybbe. Ivfvg fnccuver.ugzy -->       
```

I've done enough basic CTFs to see the word 'the' in ROT13. It was quickest to use cyberchef for this. You can also bash it out with tr.
```bash
echo 'input to encode' | tr 'A-Za-z' 'N-Zn-za-m'
```

> You get the blue gem by pushing the status to the lower floor. The gem is on the diningRoom first floor. Visit sapphire.html

With this we go to /diningRoom/sapphire.html to get the blue gem flag.

<details>
 <summary>blue gem flag</summary>
blue_jewel{******************************} 
</details>

Since we have no obvious further clue, I go back to the map and look at /tigerStatusRoom/

![](https://i.imgur.com/JTonMqH.png)

After inputting the blue gem flag we get to gem.php:

>crest 1:
>S*********************************
>Hint 1: Crest 1 has been encoded twice
>Hint 2: Crest 1 contanis 14 letters
>Note: You need to collect all 4 crests, combine and decode to reavel another path
>The combination should be crest 1 + crest 2 + crest 3 + crest 4. Also, the combination is a type of encoded base and you need to decode it 

If we try to decode it to 14 letters 

I'll look at the /galleryRoom/

![](https://i.imgur.com/wMWTFIu.png)


The note linked gives us:
>crest 2:
>GV**********************************
>Hint 1: Crest 2 has been encoded twice
>Hint 2: Crest 2 contanis 18 letters
>Note: You need to collect all 4 crests, combine and decode to reavel another path
>The combination should be crest 1 + crest 2 + crest 3 + crest 4. Also, the combination is a type of encoded base and you need to decode it

Well we haven't got any other's right now so I'll decode that when we do.

On to the next directory. /studyRoom/

![](https://i.imgur.com/SHtAA9y.png)

It's asking for a helmet flag... we we don't have one so I'll move on to the /armorRoom/

![](https://i.imgur.com/iwWfKBN.png)

It's asking for a shield flag! 

We missed something. We can try that emblem input in the /diningRoom/ with the gold_emblem flag.


>klfvg ks r wimgnd biz mpuiui ulg fiemok tqod. Xii jvmc tbkg ks tempgf ***_*****_j*****_***

After a few attempts decoding I tried multiple keys with a vigenere cypher. This was easy to crack using information we already had.

> there is a shield key inside the dining room. The html page is called ***_*****_s****_****

<details>
 <summary>shield_flag</summary>
shield_key{********************************}
</details>

we can go back to the /armorRoom/ and enter the flag.

![](https://i.imgur.com/Kyvq8ug.png)

>crest 3:
>**********************************************************************************************************************************************************************************************************************************************************=
>Hint 1: Crest 3 has been encoded three times
>Hint 2: Crest 3 contanis 19 letters
>Note: You need to collect all 4 crests, combine and decode to reavel another path
>The combination should be crest 1 + crest 2 + crest 3 + crest 4. Also, the combination is a type of encoded base and you need to decode it

Ok. so I then go to /attic/

![](https://i.imgur.com/XZh8g9Y.png)

![](https://i.imgur.com/I66d0M7.png)

>crest 4:
>gS*******************************************
>Hint 1: Crest 2 has been encoded twice
>Hint 2: Crest 2 contanis 17 characters
>Note: You need to collect all 4 crests, combine and decode to reavel another path
>The combination should be crest 1 + crest 2 + crest 3 + crest 4. Also, the combination is a type of encoded base and you need to decode it


<details>
 <summary>Crest 1</summary>
S0**************0VTUlk9 -> BASE64 DECODE -> KJW*****************RY=
KJW****************RY= -> BASE32 DECODE -> Rl************G

</details>

<details>
 <summary>Crest 2</summary>
GVFWK5KHK5********************TKE -> BASE32 DECODE -> 5Ke***************MD
5K*******************MD -> BASE58 DECODE -> ****************

</details>

<details>
 <summary>Crest 3</summary>
MDAxMTAxMTAgMDAxMTAwMTEgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAwMTEgMDAxMDAwMDAgMDAxMTAxMDAgMDExMDAxMDAgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAxMTAgMDAxMDAwMDAgMDAxMTAxMDAgMDAxMTEwMDEgMDAxMDAwMDAgMDAxMTAxMDAgMDAxMTEwMDAgMDAxMDAwMDAgMDAxMTAxMTAgMDExMDAwMTEgMDAxMDAwMDAgMDAxM********************************AxMTAgMDAxMTAxMDAgMDAxMDAwMDAgMDAxMTAxMDEgMDAxMTAxMTAgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTEwMDEgMDAxMDAwMDAgMDAxMTAxMTAgMDExMDAwMDEgMDAxMDAwMDAgMDAxMTAxMDEgMDAxMTEwMDEgMDAxMDAwMDAgMDAxMTAxMDEgMDAxMTAxMTEgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMT**********************MTAwMTEgMDAxMTAwMDAgMDAxMDAwMDAgMDAxMTAxMDEgMDAxMTEwMDAgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAwMTAgMDAxMDAwMDAgMDAxMTAxMTAgMDAxMTEwMDA= -> BASE64 DECODE -> 00110110 00110011 00100000 00110011 00****11 00100000 00110100 01100100 00100000 00****11 00110110 00100000 00110100 00111001 00100000 00110100 00111000 00100000 00110110 01100011 00100000 00110111 00110110 00100000 00110110 0*****00 00****00 001****1 00110110 001****0 00110011 00111001 001****0 00110110 01100001 00100000 00110101 00111001 00100000 00110101 00110111 00100000 00110011 00110101 00100000 00110011 00110000 00100000 00110101 00111000 00100000 00110011 00110010 00100000 00110110 00111000
00110110 00****11 00100000 00110011 00110011 00100000 00110100 01100100 00100000 00110011 00110110 00100000 00110100 00111001 00100000 00110100 00111000 00****00 00110110 01100011 001****0 00110111 00110110 00100000 00110110 00110100 00100000 00110101 00****10 001****0 00110011 00111001 00100000 00110110 01100001 00100000 00110101 00111001 00100000 001****1 00110111 00100000 00110011 00110101 00100000 00110011 00110000 00100000 00110101 00111000 00100000 00110011 00110010 00100000 00110110 0****000 -> BINARY DECODE -> 63 33 4d 36 49 48 6c 76 64 56 ** ** ** ** ** 30 58 32 68
63 33 4d 36 ** ** ** ** ** ** ** ** ** ** 35 30 58 32 68 -> HEXADECIMAL DECODE -> ****************

</details>

<details>
 <summary>Crest 4</summary>
gSUE***************************************GGYyun3k5qM9ma4s -> BASE58 DECODE -> 70 5a ** ** ** ** ** ** ** ** ** ** ** ** ** 3d 3d
70 5a ** ** ** ** ** ** ** ** ** ** ** ** ** 3d 3d -> HEXADEMICAL DECODE -> 63 33 ** ** ** ** ** ** ** ** ** ** ** 57 35 30 58 32 68
63 33 4d 36 49 ** ** ** ** ** 39 6a 59 57 35 30 58 32 68 -> HEXADECIMAL DECODE -> p************==

</details>

<details>
 <summary>Combination</summary>

RlRQIH**********1bnRlciwgRlRQIHBh****************yZXZlcg== -> BASE64 DECODE -> FTP user: ******, FTP pass: ***_****_****_*******

</details>


Now we have some ftp credentials.

```bash
$ ftp 10.10.5.66
Connected to 10.10.5.66.
220 (vsFTPd 3.0.3)
Name (10.10.5.66:speer): hunter
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 0        0            7994 Sep 19  2019 001-key.jpg
-rw-r--r--    1 0        0            2210 Sep 19  2019 002-key.jpg
-rw-r--r--    1 0        0            2146 Sep 19  2019 003-key.jpg
-rw-r--r--    1 0        0             121 Sep 19  2019 helmet_key.txt.gpg
-rw-r--r--    1 0        0             170 Sep 20  2019 important.txt
226 Directory send OK.
```


we look inside the important.txt and find a new directory /hidden_closet/ but need the helmet flag.


I used steghide to look at each image. I only got a hit with number 001 and extracted key-001.txt


I used exiftool on 002-key.jpg to see a comment on the file.

```bash
$ exiftool 002-key.jpg 
ExifTool Version Number         : 12.16
File Name                       : 002-key.jpg
Directory                       : .
File Size                       : 2.2 KiB
File Modification Date/Time     : 2021:02:22 18:18:06+00:00
File Access Date/Time           : 2021:02:22 18:20:06+00:00
File Inode Change Date/Time     : 2021:02:22 18:18:06+00:00
File Permissions                : rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
Comment                         : *****************
Image Width                     : 100
Image Height                    : 80
Encoding Process                : Progressive DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 100x80
Megapixels                      : 0.008
```

when using ```file``` and ```strings``` on the final key we can see it is a compressed file with a key-003.txt inside.

I extracted the file with ```$ binwalk -e 003-key.jpg``` and found the next part of the key.


when combined and decoded from base64 we get the text:

<details>
 <summary>decoded password</summary>
*******_***_**_*******_****_*****
</details>

I used this on the pgp file.

```bash
$ gpg helmet_key.txt.gpg
gpg: WARNING: no command supplied.  Trying to guess what you mean ...
gpg: AES256 encrypted data
gpg: encrypted with 1 passphrase

```

We got the helmet flag!

<details>
 <summary>helmet flag</summary>
helmet_key{**********************************}
</details>

![](https://i.imgur.com/i5iqYCR.png)

Both links lead to txt files
> MO Disk: wpbwbxr wpkzg pltwnhro, txrks_xfqsxrd_bvv_fy_rvmexa_ajk
This is obviously another vigenere. 
It took a few guesses but got the key to decrypt this to get Weasker's login password.

<details>
 <summary>decrypted text</summary>
weasker login password, *****_*******_***_**_******_***
</details>


>Wolfmedal: SSH password: *_*****_*****

I remembered that we also have /studyRoom/ that needed the helmet flag. 

![](https://i.imgur.com/sYLg0lH.png)

We download the file doom.tar.gz. 

After extracting the file we get eagle_medal.txt which includes the SSH username

<details>
 <summary>SSH username</summary>
*******_*****
</details>

We find a .jailcell directory within the user home directory that contains a chris.txt file.

<details>
 <summary>chris.txt</summary>
Jill: Chris, is that you?
Chris: Jill, you finally come. I was locked in the Jail cell for a while. It seem that weasker is behind all this.
Jil, What? Weasker? He is the traitor?
Chris: Yes, Jill. Unfortunately, he play us like a damn fiddle.
Jill: Let's get out of here first, I have contact brad for helicopter support.
Chris: Thanks Jill, here, take this MO Disk 2 with you. It look like the key to decipher something.
Jill: Alright, I will deal with him later.
Chris: see ya.

MO disk 2: ****** 
</details>

It tells us the vigenere password we decided to bruteforce to get the MO disk. we can now switch user to weasker.

After switching user to weasker we find a note and check his sudo privledges.
```bash
weasker@umbrella_corp:~$ sudo -l
Matching Defaults entries for weasker on umbrella_corp:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User weasker may run the following commands on umbrella_corp:
    (ALL : ALL) ALL
weasker@umbrella_corp:~$ sudo -u root ls /root/
root.txt
weasker@umbrella_corp:~$ sudo -u root cat /root/root.txt
In the state of emergency, Jill, Barry and Chris are reaching the helipad and awaiting for the helicopter support.

Suddenly, the ****** jump out from nowhere. After a tough fight, brad, throw a rocket launcher on the helipad. Without thinking twice, Jill pick up the launcher and fire at the ******.

The Tyrant shredded into pieces and the Mansion was blowed. The survivor able to escape with the helicopter and prepare for their next fight.

The End

flag: ************************
weasker@umbrella_corp:~$ 
```

Overall, I didn't feel like this box was a medium difficulty. It was a long box just because of all the decoding you had to do. But it was a nice narrative and I enjoyed it nonetheless. 
