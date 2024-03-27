# WinAntiDbg0x100

```markdown
Tags: `Reverse Engineering` `windows`
```

## **Description**

This challenge will introduce you to 'Anti-Debugging.' Malware developers don't like it when you attempt to debug their executable files because debugging these files reveals many of their secrets! That's why, they include a lot of code logic specifically designed to interfere with your debugging process. Now that you've understood the context, go ahead and debug this Windows executable! This challenge binary file is a Windows console application and you can start with running it using `cmd` on Windows. Challenge can be downloaded [here](https://artifacts.picoctf.net/c\_titan/55/WinAntiDbg0x100.zip).

<figure><img src="../.gitbook/assets/image (220).png" alt=""><figcaption></figcaption></figure>

Open it with x32dbg.

<figure><img src="../.gitbook/assets/image (221).png" alt=""><figcaption></figcaption></figure>

After analyze it. I set breakpoints as IsDebuggerPresent.

```
bp IsDebuggerPresent
```

<figure><img src="../.gitbook/assets/image (222).png" alt=""><figcaption></figcaption></figure>

Double Click IsDebuggerPresent

<figure><img src="../.gitbook/assets/image (224).png" alt=""><figcaption></figcaption></figure>

Double Click ret

<figure><img src="../.gitbook/assets/image (226).png" alt=""><figcaption></figcaption></figure>

Click step over, to see how to flow of this app work. And stop at `je winantidbgx100`

<figure><img src="../.gitbook/assets/image (225).png" alt=""><figcaption></figcaption></figure>

## **Solution Strategy**

Set breakpoints at `je winantidbgx100`

<figure><img src="../.gitbook/assets/image (227).png" alt=""><figcaption></figcaption></figure>

Change `je winantidbgx100 --> jne winantidbgx100`

<figure><img src="../.gitbook/assets/image (228).png" alt=""><figcaption></figcaption></figure>

Click step over again and again, to get the flag

## Flag

<figure><img src="../.gitbook/assets/image (219).png" alt=""><figcaption></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
