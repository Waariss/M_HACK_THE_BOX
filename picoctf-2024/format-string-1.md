# format string 1

```markdown
Tags: `Binary Exploitation` `format_string` `browser_webshell_solvable`
```

## **Description**

Patrick and Sponge Bob were really happy with those orders you made for them, but now they're curious about the secret menu. Find it, and along the way, maybe you'll find something else of interest!

<figure><img src="../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

## **Solution Strategy**

<figure><img src="../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

```python
hex_sequences = [
    "0x7b4654436f636970", 
    "0x355f31346d316e34", 
    "0x3478345f33317937", 
    "0x31655f673431665f", 
    "0x7d383130386531"    
]

decoded_strings = []
for hex_seq in hex_sequences:
    
    hex_str = hex_seq[2:].rjust((len(hex_seq[2:]) + 1) // 2 * 2, '0')
   
    decoded = bytes.fromhex(''.join(reversed([hex_str[i:i+2] for i in range(0, len(hex_str), 2)]))).decode('ascii')
    decoded_strings.append(decoded)

decoded_flag = ''.join(decoded_strings)
print(decoded_flag)
```

* **List of Hexadecimal Values**: `hex_sequences` contains hexadecimal strings likely representing parts of a message or data, potentially encoded in little-endian format.
* **Initialization of Decoded Strings List**: `decoded_strings` is initialized to store the decoded ASCII strings from the hexadecimal values.
* **Loop Through Each Hexadecimal Value**: For each hexadecimal string in `hex_sequences`:
  * **Remove '0x' Prefix and Ensure Even Length**: The hexadecimal string is processed to remove the `0x` prefix and padded to ensure it has an even length, necessary for byte conversion.
  * **Reverse Byte Order for Little-endian**: The hex string is split into bytes, and the order of these bytes is reversed to account for little-endian encoding. Little-endian means the least significant byte (LSB) comes first in the sequence, which is common in x86 architecture.
  * **Convert Hexadecimal to Bytes and Decode**: The modified hex string is converted into bytes and then decoded into ASCII characters. This step transforms the hexadecimal representation back into human-readable text.
* **Concatenate Decoded Strings**: The decoded ASCII strings are concatenated to form a single string, which is stored in `decoded_flag`.
* **Print the Decoded Flag**: Finally, the script prints the concatenated ASCII string, which represents the decoded message or data from the original list of hexadecimal values.

## Flag

<figure><img src="../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
