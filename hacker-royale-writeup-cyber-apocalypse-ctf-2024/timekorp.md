---
description: 'Category: Web Difficulty: Very Easy 300 points'
---

# TimeKORP

## Description &#x20;

Are you ready to unravel the mysteries and expose the truth hidden within KROP's digital domain? Join the challenge and prove your prowess in the world of cybersecurity. Remember, time is money, but in this case, the rewards may be far greater than you imagine.

<figure><img src="../.gitbook/assets/image (127).png" alt=""><figcaption></figcaption></figure>

### **Introduction**

In this exploration, we delve into a security analysis of the TimeKORP website.

<figure><img src="../.gitbook/assets/Screenshot from 2024-03-10 01-17-56.png" alt=""><figcaption></figcaption></figure>

### **Discovery of the Vulnerability**

The investigation began with a thorough exploration of the website's functionalities. A peculiar parameter, `format`, caught our attention, hinting at the server's processing of date formats. Initial attempts to manipulate this parameter were unfruitful until a breakthrough was achieved with the following request:&#x20;

```url
http://IP:PORT/?format=%Y-%m-%d%27$(whoami)%27
```

This request, surprisingly, returned the current user of the web server, indicating a severe command injection vulnerability. This type of vulnerability allows an attacker to execute arbitrary commands on the server, which can lead to unauthorized access to sensitive data, system compromise, and more.

<figure><img src="../.gitbook/assets/Screenshot from 2024-03-10 01-21-33.png" alt=""><figcaption></figcaption></figure>

### **Exploiting the Vulnerability**

Leveraging this vulnerability, we aimed to escalate our exploration to access sensitive data stored on the server. The next logical step was to identify and retrieve the so-called "`flag`," a common objective in cybersecurity challenges representing sensitive or secret information.

<figure><img src="../.gitbook/assets/Pasted image 20240310012416.png" alt=""><figcaption></figcaption></figure>

Given our ability to inject commands, we crafted the following request to attempt to read the contents of a file suspected to contain the flag:&#x20;

```url
http://IP:PORT/?format=%Y-%m-%d%27$(cat ../flag)%27
```

### Flag

<figure><img src="../.gitbook/assets/Screenshot from 2024-03-10 01-24-43.png" alt=""><figcaption><p>HTB{t1m3_f0r_th3_ult1m4t3_pwn4g3}</p></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
