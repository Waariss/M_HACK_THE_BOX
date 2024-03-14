---
description: 'Category: Web Difficulty: Very Easy 300 points'
---

# Flag Command

## Description &#x20;

Embark on the "Dimensional Escape Quest" where you wake up in a mysterious forest maze that's not quite of this world. Navigate singing squirrels, mischievous nymphs, and grumpy wizards in a whimsical labyrinth that may lead to otherworldly surprises. Will you conquer the enchanted maze or find yourself lost in a different dimension of magical challenges? The journey unfolds in this mystical escape!

<figure><img src="../.gitbook/assets/image (128).png" alt=""><figcaption></figcaption></figure>

### **Introduction**

The Flag Command website, at first glance, offers a straightforward user interface. However, a deeper dive into its structure and codebase reveals hidden functionalities that, when exploited, allow bypassing intended game mechanics to directly achieve the objective.

<figure><img src="../.gitbook/assets/Screenshot from 2024-03-10 00-44-39.png" alt=""><figcaption></figcaption></figure>

### **Discovery and Analysis**

The initial phase involved a meticulous inspection of the website, particularly focusing on the client-side assets. The `main.js` file, a significant part of the client-side code, hinted at an intriguing endpoint: `/api/monitor`, and a mysterious mention of "`secret`" options available through another endpoint, `/api/options`.

<figure><img src="../.gitbook/assets/Screenshot from 2024-03-10 00-45-54.png" alt=""><figcaption></figcaption></figure>

Intrigued by the possibility of hidden functionalities, the exploration continued to the `/api/options` endpoint.

<figure><img src="../.gitbook/assets/Screenshot from 2024-03-10 00-46-14.png" alt=""><figcaption></figcaption></figure>

### **Secret Command**

Navigating to `IP:PORT/api/options`, the response was a JSON object containing various parameters and values. Among these was a particularly standout entry labeled "secret". The secret command was revealed to be: "`Blip-blop, in a pickle with a hiccup! Shmiggity-shmack`".

<figure><img src="../.gitbook/assets/Screenshot from 2024-03-10 00-46-48.png" alt=""><figcaption></figcaption></figure>

### **Exploiting the Vulnerability**

The next step involved intercepting and manipulating the game's requests.

<figure><img src="../.gitbook/assets/Screenshot from 2024-03-10 00-48-39.png" alt=""><figcaption></figcaption></figure>

By injecting the discovered secret command as an input, the expectation was that the application would behave in a manner not anticipated by its regular game flow.

### Flag

<figure><img src="../.gitbook/assets/Screenshot from 2024-03-10 00-54-36.png" alt=""><figcaption><p>HTB{D3v3l0p3r_t00l5_4r3_b35t_wh4t_y0u_Th1nk}</p></figcaption></figure>

## Follow Me <a href="#follow-me" id="follow-me"></a>

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
