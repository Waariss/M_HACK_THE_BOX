# FactCheck

```markdown
Tags: `Reverse Engineering` `browser_webshell_solvable`
```

## **Description**

This binary is putting together some important piece of information... Can you uncover that information? Examine this [file](https://artifacts.picoctf.net/c\_titan/188/bin). Do you understand its inner workings?Reverse this linux executable?

<figure><img src="../.gitbook/assets/image (168).png" alt=""><figcaption></figcaption></figure>

Open it with Ghidra.

<figure><img src="../.gitbook/assets/Pasted image (38).png" alt=""><figcaption></figcaption></figure>

After analyze it. I found that they have condition.

* The condition `if v24[0] <= 65:` checks if the ASCII value of the first character in `v24` (which is "5") is less than or equal to 65. This condition evaluates to False because the ASCII value of "5" is 53, which is not less than or equal to 65.
* The condition `if v35[0] != 65:` checks if the ASCII value of the first character in `v35` (which is "6") is not equal to 65. This condition evaluates to True because the ASCII value of "6" is 54, which is not equal to 65.
* The condition `if "Hello" == "World":` checks if the string "Hello" is equal to the string "World". This condition evaluates to False since the two strings are not the same.
* The condition `if v19 - v30[0] == 3:` calculates the difference between `v19` (the ASCII value of the first character in `v26`, which is "3") and the ASCII value of the first character in `v30` (which is "e"), and checks if this difference equals 3. This condition evaluates based on the ASCII values of "3" (51) and "e" (101), which does not result in 3, so it evaluates to False.
* The condition `if v29[0] == 71:` checks if the ASCII value of the first character in `v29` (which is "a") is equal to 71. This condition evaluates to False because the ASCII value of "a" is 97, not 71.

## **Solution Strategy**

```python
def main():
    v3 = None
    v4 = None
    v5 = None
    v6 = None
    v7 = None
    v8 = None
    v9 = None
    v10 = None
    v11 = None
    v12 = None
    v13 = None
    v14 = None
    v15 = None
    v16 = None
    v17 = None
    v18 = None
    v19 = None
    v21 = bytearray()
    v22 = bytearray("picoCTF{wELF_d0N3_mate_", 'utf-8')
    v23 = bytearray("7", 'utf-8')
    v24 = bytearray("5", 'utf-8')
    v25 = bytearray("4", 'utf-8')
    v26 = bytearray("3", 'utf-8')
    v27 = bytearray("6", 'utf-8')
    v28 = bytearray("9", 'utf-8')
    v29 = bytearray("a", 'utf-8')
    v30 = bytearray("e", 'utf-8')
    v31 = bytearray("3", 'utf-8')
    v32 = bytearray("d", 'utf-8')
    v33 = bytearray("b", 'utf-8')
    v34 = bytearray("1", 'utf-8')
    v35 = bytearray("6", 'utf-8')
    v36 = bytearray("e", 'utf-8')
    v37 = bytearray("c", 'utf-8')
    v38 = bytearray("8", 'utf-8')
    v39 = None

    # Simulating readfsqword(0x28u)
    v39 = 0

    if v24[0] <= 65:
        v22.extend(v34)
    if v35[0] != 65:
        v22.extend(v37)
    if "Hello" == "World":
        v22.extend(v25)

    v19 = v26[0]
    if v19 - v30[0] == 3:
        v22.extend(v26)

    v22.extend(v25)
    v22.extend(v28)

    if v29[0] == 71:
        v22.extend(v29)

    v22.extend(v27)
    v22.extend(v36)
    v22.extend(v23)
    v22.extend(v31)
    v22.append(125)
    print(v22)

if __name__ == "__main__":
    main()
```

## Flag

<figure><img src="../.gitbook/assets/Pasted image 1 (10).png" alt=""><figcaption></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
