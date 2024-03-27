# packer

```markdown
Tags: `Reverse Engineering` `browser_webshell_solvable`
```

## **Description**

Reverse this linux executable?

```bash
strings out
```

<figure><img src="../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

```bash
upx -d out
```

<figure><img src="../.gitbook/assets/Pasted image (22).png" alt=""><figcaption></figcaption></figure>

```bash
strings out
```

<figure><img src="../.gitbook/assets/Pasted image 1 (4).png" alt=""><figcaption></figcaption></figure>

### **Solution Strategy**

```python
# Hexadecimal string from flag.txt
hex_string = "7069636f4354467b5539585f556e5034636b314e365f42316e34526933535f33373161613966667d"

# Convert the hexadecimal string to ASCII
ascii_string = bytes.fromhex(hex_string).decode()

print(ascii_string)
```

## Flag

<figure><img src="../.gitbook/assets/Pasted image 2 (6).png" alt=""><figcaption></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
