# endianness

```markdown
Tags: `General Skills` `browser_webshell_solvable`
```

## **Description**

Know of little and big endian?

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

## **Solution Strategy**

```bash
from pwn import *

def to_little_endian(word):
    return ''.join(format(ord(c), '02x') for c in reversed(word))

def to_big_endian(word):
    return ''.join(format(ord(c), '02x') for c in word)

conn = remote('titan.picoctf.net', 51008)

conn.recvuntil(b'Word: ')
word = conn.recvline().decode().strip()
print(f"Word: {word}")

little_endian = to_little_endian(word).upper()
big_endian = to_big_endian(word).upper()

conn.sendline(little_endian.encode())
print(f"Sent Little Endian: {little_endian}")

conn.recvuntil(b'Enter the Big Endian representation: ')
conn.sendline(big_endian.encode())
print(f"Sent Big Endian: {big_endian}")

response = conn.recvall().decode()
print(response)

conn.close()
```

* This Python script uses the `pwntools` library, commonly used for exploit development and CTF challenges.
* It connects to a remote server (`titan.picoctf.net`) on port `51008`.
* The `to_little_endian` function takes a word as input, converts it to little-endian hexadecimal representation, and returns the result as a string.
* The `to_big_endian` function takes a word as input, converts it to big-endian hexadecimal representation, and returns the result as a string.
* It receives a word from the server, converts it to both little-endian and big-endian representations, and sends them back to the server.
* After sending both representations, it receives the response from the server and prints it.
* Finally, it closes the connection.
* This script is likely part of a challenge where the server expects the client to convert a word into little-endian and big-endian representations and send them back.

## Flag

<figure><img src="../.gitbook/assets/Pasted image (7).png" alt=""><figcaption></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
