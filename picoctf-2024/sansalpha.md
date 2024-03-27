# SansAlpha

```markdown
Tags: `General Skills` `bash` `browser_webshell_solvable` `ssh` `shell_escape`
```

## **Description**

The Multiverse is within your grasp! Unfortunately, the server that contains the secrets of the multiverse is in a universe where keyboards only have numbers and (most) symbols.

<figure><img src="../.gitbook/assets/image (169).png" alt=""><figcaption></figcaption></figure>

## **Solution Strategy**

```bash
_1=(../../*/)
_3=("${_1[0]}"*)
_11=(*/*)
${_3[42]} ./${_11[0]}
```

* **`_1=(../../*/)`:** Stores paths of all directories two levels up from the current directory into array `_1`. Specifically targets the `bin` directory at this level.
* **`_3=("${_1[0]}"*)`:** Populates array `_3` with everything (files and directories) inside the first directory listed in `_1`, effectively capturing the contents of the `bin` directory.
* **`_11=(*/*)`:** Assigns to array `_11` paths to all items one level down from every directory in the current directory, notably includes `blargh/flag.txt`.
* **`${_3[42]} ./${_11[0]}`:** Executes the 43rd item from `_3` with `./${_11[0]}` (the first item in `_11`, `blargh/flag.txt`) as its argument. This process iterates over all items in `_3`, executing each against `blargh/flag.txt`.
* **Overall Goal:** Iteratively execute each file or script found within the `bin` directory (two levels up) against the file `blargh/flag.txt` located in a directly accessible subdirectory, aiming to identify specific behaviors or outputs.

## Flag

<figure><img src="../.gitbook/assets/image (170).png" alt=""><figcaption></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
