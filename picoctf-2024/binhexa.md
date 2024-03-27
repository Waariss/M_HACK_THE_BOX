# binhexa

```markdown
Tags: `General Skills` `browser_webshell_solvable`
```

## **Description**

How well can you perfom basic binary operations? Start searching for the flag here `nc titan.picoctf.net 50971`

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

## **Solution Strategy**

```bash
import socket
import re

def perform_operation(num1, num2, operation):
    match operation:
        case '>>':
            return num1 >> 1  
        case '<<':
            return num1 << 1  
        case '|':
            return num1 | num2
        case '&':
            return num1 & num2
        case '+':
            return num1 + num2
        case '-':
            return num1 - num2
        case '*':
            return num1 * num2
        case '/':
            return num1 // num2  
        case _:
            raise ValueError(f"Unsupported operation: {operation}")

def connect_and_solve(host, port):
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as sock:
        sock.connect((host, port))
        num1, num2 = None, None
        last_binary_result = ""

        while True:
            message = ''
            while True:
                part = sock.recv(4096).decode()
                message += part
                if "Enter the binary result:" in part or "The flag is:" in part or "Incorrect" in part or "Enter the results of the last operation in hexadecimal:" in part:
                    break
                print(part)

            new_nums = re.findall(r"Binary Number \d+: ([01]+)", message)
            if new_nums:
                num1, num2 = (int(n, 2) for n in new_nums)

            operation_search = re.search(r"Operation \d+: '([^']+)'", message)
            if operation_search:
                operation = operation_search.group(1)
                if '>>' in operation or '<<' in operation:
                    if "Binary Number 1" in message:
                        result = perform_operation(num1, 1, operation)
                    elif "Binary Number 2" in message:
                        result = perform_operation(num2, 1, operation)
                else:
                    result = perform_operation(num1, num2, operation)

                last_binary_result = bin(result)[2:]
                print("Sending:", last_binary_result)
                sock.send((last_binary_result + "\n").encode())

            if "Enter the results of the last operation in hexadecimal:" in message:
                hex_result = hex(int(last_binary_result, 2))[2:].upper()
                print("Sending in hex:", hex_result)
                sock.send((hex_result + "\n").encode())

            if "Incorrect" in message:
                print("Incorrect answer. Exiting.")
                break
            if "The flag is:" in message:
                print(message)
                break
                
if __name__ == "__main__":
    HOST = 'titan.picoctf.net'
    PORT = 64026
    connect_and_solve(HOST, PORT)
```

* This Python script connects to a server specified by the `HOST` and `PORT` constants using a TCP socket.
* It defines a function `perform_operation` to perform various bitwise and arithmetic operations on two numbers (`num1` and `num2`).
* Supported operations include shifting (`>>` and `<<`), bitwise OR (`|`), bitwise AND (`&`), addition (`+`), subtraction (`-`), multiplication (`*`), integer division (`/`), and raising a `ValueError` for unsupported operations.
* Inside `connect_and_solve` function, it establishes a connection with the server, receives messages, and parses them to extract binary numbers, operations, and expected responses.
* It continuously receives messages from the server until it receives a message indicating either to enter the binary result, enter the result of the last operation in hexadecimal, or the flag.
* It performs operations based on the received message, sends the result back to the server, and handles incorrect responses.
* When it receives the message to enter the result of the last operation in hexadecimal, it converts the last binary result to hexadecimal and sends it back.
* The main function initiates the connection and solving process when the script is executed directly.

## Flag

<figure><img src="../.gitbook/assets/Pasted image (5).png" alt=""><figcaption></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
