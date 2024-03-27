# rsa\_oracle

```markdown
Tags: `Cryptography` `browser_webshell_solvable`
```

## **Description**

Can you abuse the oracle? An attacker was able to intercept communications between a bank and a fintech company. They managed to get the [message](https://artifacts.picoctf.net/c\_titan/33/secret.enc) (ciphertext) and the [password](https://artifacts.picoctf.net/c\_titan/33/password.enc) that was used to encrypt the message. After some intensive reconassainance they found out that the bank has an oracle that was used to encrypt the password and can be found here `nc titan.picoctf.net 52816`. Decrypt the password and use it to decrypt the message. The oracle can decrypt anything except the password.

```markup
//secret.enc
Salted__ÿò(Ñè-év\rAj6<Cž·µ ^&šÕËò€±q¯\ª/KÓÃÎ:ÆÊL€õ3'~
```

```
//pwd.enc
1634668422544022562287275254811184478161245548888973650857381112077711852144181630709254123963471597994127621183174673720047559236204808750789430675058597
```

## **Solution Strategy**

```bash
#!/usr/bin/env python
from pwn import *
import subprocess

IP = 'titan.picoctf.net'
PORT = 52816

def get_target():
    return remote(IP, PORT)

with open('pwd.enc', 'r') as f:
    pas = f.read()
ipas = int(pas, 10)

t = get_target()

t.sendlineafter(b'decrypt.', b'E')
t.sendlineafter(':', p8(2))
t.recvuntil(b'(m ^ e mod n)')
c_a = int(t.recvline().strip())

t.sendlineafter(b'decrypt.', b'D')
t.sendlineafter(b':', str(c_a * ipas).encode())

t.recvuntil(b'(c ^ d mod n): ')
oracle = int(t.recvline().strip(), 0x10)

password = bytes.fromhex(hex(oracle // 2)[2:].rstrip("L")).decode()
print(f"Decrypted Password: {password}")

t.close()

command = ["openssl", "enc", "-aes-256-cbc", "-d", "-in", "secret.enc", "-k", password]
process = subprocess.Popen(command, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
stdout, stderr = process.communicate()

if process.returncode == 0:
    print("Decrypted Content:\n", stdout.decode())
else:
    print("Decryption failed:", stderr.decode())
```

* **Import and Setup**
  * The script begins by importing necessary libraries: `pwntools` for network communication and `subprocess` for executing external commands.
  * The target encryption service is defined with its IP (`titan.picoctf.net`) and port (`52816`), indicating the script is designed to interact with a specific remote service.
* **Reading Encrypted Password**
  * An encrypted password is read from a file named `pwd.enc`. This file contains the password needed to decrypt another file, but in encrypted form. The script reads this encrypted password and converts it to an integer (`ipas`), preparing it for further operations.
* Establishing Connection
  * Utilizing `pwntools`, the script establishes a network connection to the remote encryption service. This connection is used to send commands to and receive responses from the service, facilitating remote exploitation.
* **Exploiting Encryption**
  * **Encryption Request:** The script first instructs the remote service to encrypt the number `2`. This step is crucial because it uses the service's own encryption functionality against it. The response, `c_a`, is the encrypted form of `2` according to the service's encryption scheme.
  * **Crafting Decryption Request:** Next, the script sends a decryption request, but with a twist. Instead of asking to decrypt a straightforward message, it requests the decryption of the product of `c_a` (the encrypted `2`) and `ipas` (the encrypted password, now as an integer). This crafted request exploits specific vulnerabilities in the encryption algorithm's implementation, leveraging mathematical properties of RSA encryption to manipulate the service into performing operations that ultimately aid in reversing the encryption.
* **Decrypting the Password**
  * **Processing the Decryption Response:** The service responds with `oracle`, the result of the decryption request. The script processes this response to extract the original password. By dividing `oracle` by `2` and converting it back from hexadecimal, the script effectively reverses the encryption applied to the original password. This step cleverly uses the properties of RSA encryption, where specific manipulations of encrypted data can yield useful decrypted outcomes.
* **File Decryption**
  * **Decrypting `secret.enc`:** With the decrypted password now available, the script proceeds to decrypt `secret.enc` using OpenSSL. This step is straightforward: it runs an OpenSSL command with the AES-256-CBC decryption option, using the decrypted password as the key. The success of this operation hinges on correctly extracting the password in the previous steps.
* **Output Handling**
  * The script evaluates the success of the decryption operation by checking the return code of the OpenSSL command. A return code of `0` indicates success, prompting the script to print the decrypted contents of `secret.enc`. If the operation fails, an error message is displayed instead.

## Flag

<figure><img src="../.gitbook/assets/Pasted image (18).png" alt=""><figcaption></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
