# interencdec

```markdown
Tags: `Cryptography` `base64` `browser_webshell_solvable` `caesar`
```

## **Description**

Can you get the real meaning from this file.

<pre class="language-bash"><code class="lang-bash">//enc_flag
<strong>YidkM0JxZGtwQlRYdHFhR3g2YUhsZmF6TnFlVGwzWVROclh6YzRNalV3YUcxcWZRPT0nCg==
</strong></code></pre>

## **Solution Strategy**

```bash
import base64

encoded_base64 = "YidkM0JxZGtwQlRYdHFhR3g2YUhsZmF6TnFlVGwzWVROclh6YzRNalV3YUcxcWZRPT0nCg=="

print("Encoded base64 string:", encoded_base64)

decoded_bytes = base64.b64decode(encoded_base64)
decoded_string = decoded_bytes.decode("utf-8")

print("Decoded base64 string:", decoded_string)

import re
base64_pattern = "'(.*?)'"
base64_match = re.search(base64_pattern, decoded_string)
if base64_match:
    base64_text = base64_match.group(1)
else:
    raise ValueError("No base64 string found within single quotes")

print("Extracted base64 string:", base64_text)

decoded_text_bytes = base64.b64decode(base64_text)
decoded_text = decoded_text_bytes.decode("utf-8")

print("Decoded base64 text before shift:", decoded_text)

def caesar_cipher(text, shift):
    result = ""
    for char in text:
        if char.isalpha():
            shifted = ord(char) + shift
            if char.islower():
                if shifted > ord("z"):
                    shifted -= 26
                elif shifted < ord("a"):
                    shifted += 26
            elif char.isupper():
                if shifted > ord("Z"):
                    shifted -= 26
                elif shifted < ord("A"):
                    shifted += 26
            result += chr(shifted)
        else:
            result += char
    return result

shifted_text = caesar_cipher(decoded_text, 19)

print("Decoded and shifted base64 text:", shifted_text)
```

* The script decodes a base64-encoded string (`encoded_base64`) using the `base64` module and prints the decoded string.
* It then extracts a substring from the decoded string using a regular expression pattern to find a base64 string within single quotes.
* After decoding this extracted base64 string, it applies a Caesar Cipher with a shift of 19 to the decoded text using a custom function (`caesar_cipher`).
* Finally, it prints the resulting text after applying the Caesar Cipher.
* The overall purpose seems to be to decode a base64-encoded string, extract another base64 string from it, decode it, and then apply a Caesar Cipher with a specific shift to it.

## Flag

<figure><img src="../.gitbook/assets/Pasted image (8).png" alt=""><figcaption></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
