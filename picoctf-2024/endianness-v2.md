# endianness-v2

```markdown
Tags: `Forensics` `browser_webshell_solvable`
```

## **Description**

Here's a file that was recovered from a 32-bits system that organized the bytes a weird way. We're not even sure what type of file it is.

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

```sh
hexedit challengefile
```

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

## **Solution Strategy**

```python
def correct_byte_order(input_file_path, output_file_path):
    """
    Corrects the byte order within each 32-bit word of the file.

    Parameters:
    - input_file_path: Path to the input file with incorrect byte order.
    - output_file_path: Path where the corrected file will be saved.
    """
    corrected_bytes = bytearray()
    
    with open(input_file_path, 'rb') as file:
        while True:
            word = file.read(4)
            if not word:
                break
            corrected_bytes.extend(word[::-1])
    
    with open(output_file_path, 'wb') as corrected_file:
        corrected_file.write(corrected_bytes)

input_file_path = 'challengefile'
output_file_path = 'fixed_challengefile.jpeg'
correct_byte_order(input_file_path, output_file_path)

print(f'Corrected file has been saved to: {output_file_path}')
```

* The `correct_byte_order` function corrects the byte order within each 32-bit word of a file.
* It takes two parameters: `input_file_path` (path to the input file with incorrect byte order) and `output_file_path` (path where the corrected file will be saved).
* Inside the function, it initializes an empty `bytearray` to store the corrected bytes.
* It then opens the input file in binary mode and iterates over the file content in chunks of 4 bytes.
* For each 32-bit word read from the file, it reverses the byte order (from little-endian to big-endian or vice versa) and appends it to the `corrected_bytes` bytearray.
* After processing the entire input file, it opens the output file in binary write mode and writes the corrected bytes to it.
* Finally, an example usage of the function is provided where it takes an input file named `'challengefile'`, corrects the byte order, and saves the corrected file as `'fixed_challengefile.jpeg'`.
* Upon execution, it prints a message indicating the path where the corrected file has been saved.

## Flag

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
