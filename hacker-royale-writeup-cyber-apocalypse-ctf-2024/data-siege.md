---
description: 'Category: Forensics Difficulty: Medium  300 points'
---

# Data Siege

### Description

"It was a tranquil night in the Phreaks headquarters, when the entire district erupted in chaos. Unknown assailants, rumored to be a rogue foreign faction, have infiltrated the city's messaging system and critical infrastructure. Garbled transmissions crackle through the airwaves, spewing misinformation and disrupting communication channels. We need to understand which data has been obtained from this attack to reclaim control of the and communication backbone. Note: flag is splitted in three parts."

<figure><img src="../.gitbook/assets/Pasted image (27).png" alt=""><figcaption></figcaption></figure>

### **Introduction**

The investigation kicked off by diving into a `capture.pcap` file through Wireshark. This step was critical for breaking down the network's exported objects and communications.

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

### **Deciphering Conversations**

<figure><img src="../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>

In the web of TCP streams, one conversation stood out due to its encryption methods - a blend of AES and base64. This interaction suggested that hidden messages were waiting to be uncovered.&#x20;

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

The use of Burp Suite to decode the base64 segments led to an exciting find: readable text revealing the third part of a flag.

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

### **Exported Objects Analysis**

A deeper look into the HTTP exported objects unearthed three significant files.&#x20;

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

Among these, two instances of "nBISC4YJKs7j4I.xml" pointed towards the "aQ4caZ.exe" file.

<figure><img src="../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

### **Decrypting the Code**

<figure><img src="../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

Decompiling the "aQ4caZ.exe" file with dnSpy was the next step, which exposed the encryption key. &#x20;

<figure><img src="../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

Further exploration of the code revealed a decryption function with a hard-coded salt value.

<figure><img src="../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

This discovery paved the way to rewrite the decryption function in Python, using the uncovered key.

```python
from Crypto.Cipher import AES
from Crypto.Protocol.KDF import PBKDF2
from Crypto.Util.Padding import unpad
import base64

def decrypt(cipher_text, encrypt_key):
    try:
    
        salt = bytes([86, 101, 114, 121, 95, 83, 51, 99, 114, 51, 116, 95, 83])
        
        
        key_iv = PBKDF2(encrypt_key, salt, dkLen=48, count=1000) 
        key = key_iv[:32]
        iv = key_iv[32:48]
        
        
        cipher_data = base64.b64decode(cipher_text)
        
        
        cipher = AES.new(key, AES.MODE_CBC, iv)
        decrypted = unpad(cipher.decrypt(cipher_data), AES.block_size)
        
        return decrypted.decode('utf-8') 
    except Exception as e:
        print(f"Error during decryption: {e}")
        return "error"

encrypt_key = "VYAemVeO3zUDTL6N62kVA"
cipher_text = "zVmhuROwQw02oztmJNCvd2v8wXTNUWmU3zkKDpUBqUON+hKOocQYLG0pOhERLdHDS+yw3KU6RD9Y4LDBjgKeQnjml4XQMYhl6AFyjBOJpA4UEo2fALsqvbU4Doyb/gtg"
print(decrypt(cipher_text, encrypt_key))
```

* **Import Necessary Modules**: It starts by importing the necessary modules from the `pycryptodome` library, which provides cryptographic operations like AES encryption and decryption, padding mechanisms, and key derivation functions.
* **Define the `decrypt` function**: This function is responsible for decrypting a given piece of cipher text using a specified encryption key.
  * **Salt**: A predefined salt (`salt = bytes([...])`) is used, which is essential in the key derivation process to produce a unique key based on the encryption key and salt.
  * **Key Derivation**: It uses the `PBKDF2` (Password-Based Key Derivation Function 2) algorithm to derive a 48-byte key and initialization vector (IV) from the encryption key and salt. The `dkLen=48` specifies the desired length of the derived key, and `count=1000` specifies the iteration count, affecting the computation time and security level.
  * **Key and IV**: The first 32 bytes of the derived data are used as the AES key, and the next 16 bytes are used as the IV for the CBC mode.
  * **Base64 Decoding**: The cipher text, assumed to be base64 encoded, is decoded back into its original binary form before decryption.
  * **Decryption**: An AES cipher is created using the derived key and IV, and the cipher text is decrypted. The `unpad` function is then used to remove any padding added to the message during encryption, ensuring the plaintext is restored to its original form.
  * **Decoding**: The decrypted data is assumed to be UTF-8 encoded and is decoded back into a string.
  * **Error Handling**: The function includes error handling to catch and report any issues that occur during the decryption process.

### **Flag**

With the decryption function ready, applying it to the previously found encrypted communications was the final hurdle. This effort was rewarded with the discovery of the first and second parts of the flag, piecing together the puzzle.

<figure><img src="../.gitbook/assets/image (10) (1).png" alt=""><figcaption><p>HTB{c0mmun1c4710n5_h45_b33n_r3570r3d_1n_7h3_h34dqu4r73r5}</p></figcaption></figure>

## Follow Me <a href="#follow-me" id="follow-me"></a>

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
