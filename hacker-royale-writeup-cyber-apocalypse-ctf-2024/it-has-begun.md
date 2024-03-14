---
description: 'Category: Forensics Difficulty: Very Easy 300 points'
---

# It Has Begun

## Description &#x20;

The Fray is upon us, and the very first challenge has been released! Are you ready factions!? Considering this is just the beginning, if you cannot musted the teamwork needed this early, then your doom is likely inevitable.

<figure><img src="../.gitbook/assets/image (131).png" alt=""><figcaption></figcaption></figure>

### **Introduction**

We began by analyzing a shell script associated with "**KORP-STATION-013.**" This script was designed to terminate certain processes and set up a connection for future use, indicating the start of covert operations.

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

### Discovery

An interesting part of the script was an `echo` command that added an SSH key to a file. This key contained a string that seemed out of place. By reversing this string, we found the first part of a hidden message within the SSH key.

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

### Deep Dive

Further inspection of the script revealed an encoded message intended for the `crontab`, a scheduler in Unix systems.&#x20;

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

Decoding this base64-encoded string gave us the second piece of the puzzle, revealing the final part of the message.

### Flag

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption><p>HTB{w1ll_y0u_St4nd_y0uR_Gr0uNd!!}</p></figcaption></figure>

## Follow Me <a href="#follow-me" id="follow-me"></a>

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
