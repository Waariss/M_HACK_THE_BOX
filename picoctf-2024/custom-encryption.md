# Custom encryption

```markdown
Tags: `Cryptography` `ASCII_encoding` `browser_webshell_solvable` `XOR`
```

## **Description**

Can you get sense of this code file and write the function that will decode the given encrypted file content. Find the encrypted file here [flag\_info](https://artifacts.picoctf.net/c\_titan/17/enc\_flag) and [code file](https://artifacts.picoctf.net/c\_titan/17/custom\_encryption.py) might be good to analyze and get the flag.

```python
//custom_encryption.py
from random import randint
import sys


def generator(g, x, p):
    return pow(g, x) % p


def encrypt(plaintext, key):
    cipher = []
    for char in plaintext:
        cipher.append(((ord(char) * key*311)))
    return cipher


def is_prime(p):
    v = 0
    for i in range(2, p + 1):
        if p % i == 0:
            v = v + 1
    if v > 1:
        return False
    else:
        return True


def dynamic_xor_encrypt(plaintext, text_key):
    cipher_text = ""
    key_length = len(text_key)
    for i, char in enumerate(plaintext[::-1]):
        key_char = text_key[i % key_length]
        encrypted_char = chr(ord(char) ^ ord(key_char))
        cipher_text += encrypted_char
    return cipher_text


def test(plain_text, text_key):
    p = 97
    g = 31
    if not is_prime(p) and not is_prime(g):
        print("Enter prime numbers")
        return
    a = randint(p-10, p)
    b = randint(g-10, g)
    print(f"a = {a}")
    print(f"b = {b}")
    u = generator(g, a, p)
    v = generator(g, b, p)
    key = generator(v, a, p)
    b_key = generator(u, b, p)
    shared_key = None
    if key == b_key:
        shared_key = key
    else:
        print("Invalid key")
        return
    semi_cipher = dynamic_xor_encrypt(plain_text, text_key)
    cipher = encrypt(semi_cipher, shared_key)
    print(f'cipher is: {cipher}')


if __name__ == "__main__":
    message = sys.argv[1]
    test(message, "trudeau")
```

```bash
//enc_flag
a = 94
b = 21
cipher is: [131553, 993956, 964722, 1359381, 43851, 1169360, 950105, 321574, 1081658, 613914, 0, 1213211, 306957, 73085, 993956, 0, 321574, 1257062, 14617, 906254, 350808, 394659, 87702, 87702, 248489, 87702, 380042, 745467, 467744, 716233, 380042, 102319, 175404, 248489]
```

## **Solution Strategy**

```bash
def decrypt(ciphertext, key):
    plaintext = ""
    for num in ciphertext:
        decrypted_num = num // (key * 311)
        plaintext += chr(decrypted_num)
    return plaintext

def dynamic_xor_decrypt(ciphertext, text_key):
    decrypted_text = ""
    key_length = len(text_key)
    for i, char in enumerate(ciphertext):
        key_char = text_key[i % key_length]
        decrypted_char = chr(ord(char) ^ ord(key_char))
        decrypted_text += decrypted_char
    return decrypted_text

def find_shared_key(p, g, a, b):
    u = pow(g, a, p)
    v = pow(g, b, p)
    shared_key = pow(v, a, p)
    b_key = pow(u, b, p)
    if shared_key == b_key:
        return shared_key
    else:
        return None

def main():
    p = 97
    g = 31
    a = 94
    b = 21
    shared_key = find_shared_key(p, g, a, b)
    if shared_key is None:
        print("Invalid key")
        return
    
    ciphertext = [131553, 993956, 964722, 1359381, 43851, 1169360, 950105, 321574, 1081658, 613914, 0, 1213211, 306957, 73085, 993956, 0, 321574, 1257062, 14617, 906254, 350808, 394659, 87702, 87702, 248489, 87702, 380042, 745467, 467744, 716233, 380042, 102319, 175404, 248489]
    decrypted_text = decrypt(ciphertext, shared_key)
    decrypted_text = dynamic_xor_decrypt(decrypted_text, "trudeau")
    decrypted_text = decrypted_text[::-1]
    print("Decrypted text:", decrypted_text)

if __name__ == "__main__":
    main()

```

* The `decrypt` function takes a list of ciphertext numbers and a key as input, then decrypts the ciphertext by dividing each number by the product of the key and a constant (`311`), converting the decrypted numbers to characters, and appending them to form the plaintext.
* The `dynamic_xor_decrypt` function performs a dynamic XOR decryption on a ciphertext using a text-based key. It XORs each character of the ciphertext with the corresponding character from the key, repeating the key cyclically if necessary, and builds the decrypted text.
* The `find_shared_key` function calculates a shared key using Diffie-Hellman key exchange algorithm parameters (`p`, `g`) and private keys (`a`, `b`). It computes intermediate values `u` and `v`, then derives the shared key using modular exponentiation.
* In the `main` function, it initializes parameters for Diffie-Hellman key exchange (`p`, `g`, `a`, `b`) and calculates the shared key using `find_shared_key`.
* It then decrypts a ciphertext list using the shared key and the `decrypt` function, followed by a dynamic XOR decryption using the `dynamic_xor_decrypt` function with the key `"trudeau"`.
* Finally, it reverses the decrypted text and prints the result.

## Flag

<figure><img src="../.gitbook/assets/Pasted image (9).png" alt=""><figcaption></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
