# Mob psycho

```markdown
Tags: `Forensics` `browser_webshell_solvable` `apk`
```

## **Description**

Can you handle APKs?

```sh
unzip mobpsycho.apk
```

<figure><img src="../.gitbook/assets/Pasted image (14).png" alt=""><figcaption></figcaption></figure>

```bash
find /home/waaris_m/Downloads/picoCTF_2024/Mob_psycho/ -type f -name "flag.*"

cat /home/waaris_m/Downloads/picoCTF_2024/Mob_psycho/res/color/flag.txt
```

<figure><img src="../.gitbook/assets/Pasted image 1 (1).png" alt=""><figcaption></figcaption></figure>

## **Solution Strategy**

```python
# Hexadecimal string from flag.txt
hex_string = "7069636f4354467b6178386d433052553676655f4e5838356c346178386d436c5f38356462643231357d"

# Convert the hexadecimal string to ASCII
ascii_string = bytes.fromhex(hex_string).decode()

print(ascii_string)
```

## Flag

<figure><img src="../.gitbook/assets/Pasted image 2.png" alt=""><figcaption></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
