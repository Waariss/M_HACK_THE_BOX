---
description: 'Category: Forensics Difficulty: Easy 300 points'
---

# Fake Boost

## Description &#x20;

In the shadow of The Fray, a new test called ""Fake Boost"" whispers promises of free Discord Nitro perks. It's a trap, set in a world where nothing comes without a cost. As factions clash and alliances shift, the truth behind Fake Boost could be the key to survival or downfall. Will your faction see through the deception? KORP™ challenges you to discern reality from illusion in this cunning trial.

<figure><img src="../.gitbook/assets/image (132).png" alt=""><figcaption><p>I gave this flag to my friend</p></figcaption></figure>

### **Introduction**

After downloading a `capture.pcapng` file and opening it with Wireshark, the focus was to examine the exported objects within this network communication.&#x20;

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

This exploration led to the discovery of three distinct files.

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

### **Discovery of Files**

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

**freediscordnitro**: This file was full of base64 characters, which initially seemed undecodable. Opting to move forward, it was set aside for later examination.

<figure><img src="../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

**rj1893rj1joijdkajwda**: Enclosed within this file were characters that appeared to be decode with base 64 and encrypted with AES.

<figure><img src="../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

**rj1893rj1joijdkajwda(1)**: Merely containing the text "OK," it didn't present anything of immediate interest.

<figure><img src="../.gitbook/assets/image (136).png" alt=""><figcaption></figcaption></figure>

### **Decoding Attempts and Successes**

Persisting with the `freediscordnitro` file, a breakthrough was achieved by reversing the base64 text before attempting to decode it again.

<figure><img src="../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

Utilizing Burp Suite for the decoding process unveiled it as an AES encryption code alongside a Free Discord Nitro 2024 offer, which also included a part of the flag encoded in base64.

<figure><img src="../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

Decoding this base64 text revealed the first segment of the flag.

<figure><img src="../.gitbook/assets/part1.png" alt=""><figcaption></figcaption></figure>

### **Unlocking the AES-Encrypted File**

Further analysis led to the discovery of the AES key.

<figure><img src="../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

The next step involved decoding the contents of the `rj1893rj1joijdkajwda` file from base64, followed by decrypting the text using the AES key.

<figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

* **Import AES Cipher and base64 Decoding**: The script begins by importing the necessary components for AES decryption from the `Crypto.Cipher` module and base64 decoding functionality.
* **Decode AES Key**: It decodes the AES key from base64 format to bytes, making it usable for decryption.
* **Decode Encrypted Data**: Similarly, the encrypted data is also base64 decoded back into its original binary format.
* **Extract IV and Ciphertext**: The initial vector (IV) is assumed to be the first 16 bytes of the encrypted data, with the remainder being the actual ciphertext. This IV is used to ensure that the encryption of similar plaintext blocks results in different ciphertexts.
* **Initialize AES Cipher**: An AES cipher object is created with the decoded AES key, set to use CBC (Cipher Block Chaining) mode, and initialized with the extracted IV.
* **Decrypt Data**: The ciphertext is decrypted using the initialized cipher, resulting in the original plaintext but potentially with added padding.
* **Decode and Clean Decrypted Data**: Attempts to decode the decrypted data into a readable string format. It removes padding manually by stripping specific padding bytes (`\x0c`, `\x0b`, `\x0a`, etc.) from the end of the plaintext. This manual approach to padding removal is not standard and assumes a specific padding pattern that may not apply to all encryption scenarios.
* **Error Handling**: If decoding fails (e.g., due to incorrect padding or encoding issues), it catches the exception and prints an error message.

This decryption process unearthed details including an ID, Email, GlobalName, and Token.

<figure><img src="../.gitbook/assets/AES.png" alt=""><figcaption></figcaption></figure>

### Flag

Focusing on the EMAIL and decoding it resulted in uncovering the second part of the flag.

<figure><img src="../.gitbook/assets/flag_2.png" alt=""><figcaption><p>HTB{fr33_N17r0G3n_3xp053d!_b3W4r3_0f_T00_g00d_2_b3_7ru3_0ff3r5}</p></figcaption></figure>

## Follow Me <a href="#follow-me" id="follow-me"></a>

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
