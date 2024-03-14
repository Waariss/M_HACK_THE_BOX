---
description: 'Category: Crypto Difficulty: Very Easy 300 points'
---

# Primary Knowledge

## Description &#x20;

Surrounded by an untamed forest and the serene waters of the Primus river, your sole objective is surviving for 24 hours. Yet, survival is far from guaranteed as the area is full of Rattlesnakes, Spiders and Alligators and the weather fluctuates unpredictably, shifting from scorching heat to torrential downpours with each passing hour. Threat is compounded by the existence of a virtual circle which shrinks every minute that passes. Anything caught beyond its bounds, is consumed by flames, leaving only ashes in its wake. As the time sleeps away, you need to prioritise your actions secure your surviving tools. Every decision becomes a matter of life and death. Will you focus on securing a shelter to sleep, protect yourself against the dangers of the wilderness, or seek out means of navigating the Primus’ waters?

<figure><img src="../.gitbook/assets/image (133).png" alt=""><figcaption></figcaption></figure>

### Introduction

The script, elegant in its simplicity, was designed to encrypt a message — the FLAG — using the RSA encryption algorithm. The cornerstone of RSA is the product of two prime numbers to form `n`, a crucial part of the public key. However, this script deviated from the established path.

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption><p>This is a output.txt </p></figcaption></figure>

### The Revelation

Thanks P'North at KPMG Thailand for give me a hints, a helpful on Stack Exchange([https://math.stackexchange.com/questions/1077411/textbook-rsa-game-with-one-prime](https://math.stackexchange.com/questions/1077411/textbook-rsa-game-with-one-prime))

It became clear that the encryption used only one prime instead of two. This was not the unassailable RSA known to many but a variant weaker by design or mistake. With only one prime to contend with, the decryption became a matter not of factoring but of taking roots.

<figure><img src="../.gitbook/assets/post.png" alt=""><figcaption></figcaption></figure>

### The Decryption

The script to decrypt was simple: calculate the `e`th root of `c` modulo `n`. Since `n` was prime, the modulo operation was straightforward, and the root revealed itself as the clear text of the flag.

<figure><img src="../.gitbook/assets/decode.png" alt=""><figcaption></figcaption></figure>

* **Importing sympy:** The code starts by importing the `sympy` library, which provides tools for symbolic mathematics in Python, including functions relevant to number theory.
* **Given values:** The encryption exponent `e`, the ciphertext `c`, and the modulus `n` are provided. These are typically part of the public key in RSA, with `n` being the product of two primes (which should be the case but isn't here due to a mistake).
* **Calculation of `d`:** The script calculates the private exponent `d` by finding the modular multiplicative inverse of `e` modulo `phi(n)`, where `phi(n)` is the Euler's totient of `n`. Since the `n` in the script is prime, `phi(n)` is simply `n - 1`.
* **Decryption of the message:** The script then decrypts the ciphertext `c` by raising it to the power of `d` modulo `n`, which is the standard decryption process in RSA.
* **Conversion to bytes:** The decrypted message `m` is then converted from a number to a sequence of bytes, which is supposed to be the original plaintext message.
* **Decoding and printing:** Finally, the byte sequence is decoded from its byte representation into a human-readable string, which is printed out. This decoded string is expected to be the flag that was encrypted.

### Flag

<figure><img src="../.gitbook/assets/flag (1).png" alt=""><figcaption></figcaption></figure>

## Follow Me <a href="#follow-me" id="follow-me"></a>

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
