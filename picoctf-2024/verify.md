# Verify

```markdown
Tags: `Forensics` `checksum` `browser_webshell_solvable` `grep`
```

## **Description**

People keep trying to trick my players with imitation flags. I want to make sure they get the real thing! I'm going to provide the SHA-256 hash and a decrypt script to help you know that my flags are legitimate.

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

```sh
#decrypt.sh
        #!/bin/bash

        # Check if the user provided a file name as an argument
        if [ $# -eq 0 ]; then
            echo "Expected usage: decrypt.sh <filename>"
            exit 1
        fi

        # Store the provided filename in a variable
        file_name="$1"

        # Check if the provided argument is a file and not a folder
        if [ ! -f "/home/ctf-player/drop-in/$file_name" ]; then
            echo "Error: '$file_name' is not a valid file. Look inside the 'files' folder with 'ls -R'!"
            exit 1
        fi

        # If there's an error reading the file, print an error message
        if ! openssl enc -d -aes-256-cbc -pbkdf2 -iter 100000 -salt -in "/home/ctf-player/drop-in/$file_name" -k picoCTF; then
            echo "Error: Failed to decrypt '$file_name'. This flag is fake! Keep looking!"
        fi
```

## **Solution Strategy**

```python
import os
import subprocess
import sys

if len(sys.argv) != 2:
    print("Expected usage: decrypt.py <directory_path>")
    sys.exit(1)

directory_path = sys.argv[1]

if not os.path.isdir(directory_path):
    print(f"Error: '{directory_path}' is not a valid directory. Provide a valid directory path!")
    sys.exit(1)

for file_name in os.listdir(directory_path):
    full_path = os.path.join(directory_path, file_name)
    if os.path.isfile(full_path):
        print(f"Attempting to decrypt: {file_name}")
        try:
            subprocess.run(['openssl', 'enc', '-d', '-aes-256-cbc', '-pbkdf2', '-iter', '100000', '-salt', 
                            '-in', full_path, '-k', 'picoCTF'], check=True)
            print(f"Decrypted: {file_name}")
        except subprocess.CalledProcessError:
            print(f"Error: Failed to decrypt '{file_name}'. This flag might be fake or the file is not encrypted with the expected parameters!")

print("Finished processing all files.")
```

* This script is designed to decrypt files within a specified directory using OpenSSL.
* It expects one command-line argument, which should be the path to the directory containing the files to decrypt.
* If the argument count is not exactly 2, it prints an error message and exits with a status code of 1.
* It checks if the provided directory path exists and is indeed a directory; if not, it prints an error message and exits with a status code of 1.
* It iterates over each file in the directory, attempting to decrypt each file using OpenSSL with AES-256 CBC encryption, PBKDF2 key derivation, 100000 iterations, and a salt.
* If decryption is successful, it prints a success message; otherwise, it prints an error message indicating the failure to decrypt the file.
* Finally, it prints a message indicating that it has finished processing all files in the directory.

## Flag

<figure><img src="../.gitbook/assets/Pasted image (10).png" alt=""><figcaption></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
