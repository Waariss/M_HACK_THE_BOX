---
description: 'Category: Misc Difficulty: Very Easy 300 points'
---

# Stop Drop and Roll

## Description &#x20;

The Fray: The Video Game is one of the greatest hits of the last... well, we don't remember quite how long. Our "computers" these days can't run much more than that, and it has a tendency to get repetitive...

<figure><img src="../.gitbook/assets/image (130).png" alt=""><figcaption></figcaption></figure>

### **Introduction**

In the realm of cyber competitions, challenges often simulate realistic scenarios requiring quick thinking and automation skills. "THE FRAY: THE VIDEO GAME" challenge was an exemplary test of such skills, where participants had to interact with a remote service to prove their mettle. The game provided scenarios requiring precise responses to conquer the virtual "GAUNTLET".

### **Challenge Breakdown**

Upon connecting to the service via `netcat`, the game described three distinct scenarios — GORGE, PHREAK, or FIRE. Each scenario necessitated a specific response:

* GORGE required the participant to respond with STOP.
* PHREAK necessitated a ROLL response.
* FIRE called for the DROP response.

The twist came with combined scenarios, where multiple commands had to be sent back in sequence.

<figure><img src="../.gitbook/assets/Pasted image (2).png" alt=""><figcaption></figcaption></figure>

### **Solution Strategy**

To tackle this challenge efficiently, manual interaction was not practical. Thus, a Python script was developed to automate the communication with the service. The script's algorithm was straightforward but effective:

* Connect to the remote service using the socket library.
* Listen for the challenge's prompt.
* Parse the received message and determine the appropriate response based on the rules.
* Send the response back to the server.
* Repeat the process until the server sent the flag, signaling the challenge's completion.

<figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

### **Flag**

Through the use of Python for automation, the responses were sent accurately and swiftly, satisfying the conditions set forth by the game.

<figure><img src="../.gitbook/assets/flag (2).png" alt=""><figcaption><p>HTB{1_wiLl_sT0p_dR0p_4nD_r0Ll_mY_w4Y_oUt!}</p></figcaption></figure>

## Follow Me <a href="#follow-me" id="follow-me"></a>

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
