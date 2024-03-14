---
description: 'Category: Crypto Difficulty: Easy 300 points'
---

# Blunt

## Description &#x20;

Valuing your life, you evade the other parties as much as you can, forsaking the piles of weaponry and the vantage points in favour of the depths of the jungle. As you jump through the trees and evade the traps lining the forest floor, a glint of metal catches your eye. Cautious, you creep around, careful not to trigger any sensors. Lying there is a knife - damaged and blunt, but a knife nonetheless. You’re not helpless any more.

<figure><img src="../.gitbook/assets/image (134).png" alt=""><figcaption></figcaption></figure>

### Introduction

The challenge unraveled a cryptic tale involving the Diffie-Hellman key exchange, a ceremony where two entities conjure a shared secret without ever exchanging the secret itself. The sage script provided to participants utilized this arcane ritual to encrypt a message — the FLAG — with AES in CBC mode, leaving behind only the prime _p_, the generator _g_, and the magical numbers _A_ and _B_, along with a ciphertext and an initialization vector (IV).

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption><p>The output.txt</p></figcaption></figure>

### Algorithm

Baby-Step Giant-Step algorithm, known in discrete logarithms within polynomial time, provided a hope in the mystery of _B_ and discovering the elusive exponent _b_.  ([https://github.com/ashutosh1206/Crypton/blob/master/Discrete-Logarithm-Problem/Algo-Baby-Step-Giant-Step/README.md](https://github.com/ashutosh1206/Crypton/blob/master/Discrete-Logarithm-Problem/Algo-Baby-Step-Giant-Step/README.md))

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

### The Solution

Implementing the Baby-Step Giant-Step algorithm revealed the exponent _b_, a critical piece in the puzzle. With _b_ in hand, the shared secret _C_ was summoned using the powers of _A_ and _b_ within the prime confines of _p_. This secret, once a mere whisper in the wind, was then transformed through the alchemy of SHA-256 into a key, unlocking the ancient AES encryption.

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

* **Imports cryptographic and mathematical libraries:** Utilizes `Crypto.Cipher` for AES encryption, `Crypto.Util.Padding` for padding related functions, `Crypto.Util.number` for number conversions, `hashlib` for SHA-256 hashing, and `math` for mathematical operations.
* **Initial values setup:** Defines the prime `p`, generator `g`, public values `A` and `B`, the ciphertext, and the initialization vector (IV) used for AES decryption.
* **Baby-Step Giant-Step algorithm:** Implements this algorithm to solve for `b`, the discrete logarithm problem of finding the exponent given `g`, `B`, and `p`. This is necessary because `b` is not directly provided but is crucial for deriving the shared secret `C`.
  * **Setup a dictionary of baby steps:** For integers up to the square root of `p`, calculate `g^i mod p` and store in a dictionary.
  * **Use the giant step:** Calculate inverses of `g` raised to multiples of the square root of `p` and check against the baby steps to find `b`.
* **Calculates the shared secret `C`:** Uses the found value of `b` to compute the shared secret by calculating `A^b mod p`.
* **Derives the AES key:** Hashes the shared secret `C` using SHA-256 and takes the first 16 bytes of the hash as the AES key.
* **Decrypts the ciphertext:** Uses AES in CBC mode with the derived key and the given IV to decrypt the ciphertext. The result is unpadded to remove padding added during encryption.
* **Outputs the decrypted message:** If `b` is successfully found and the decryption proceeds, prints the decrypted plaintext. If `b` cannot be found, prints a failure message.

### Flag

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

## Follow Me <a href="#follow-me" id="follow-me"></a>

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
