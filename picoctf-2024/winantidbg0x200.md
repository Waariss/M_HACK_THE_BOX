# WinAntiDbg0x200

```markdown
Tags: `Reverse Engineering` `windows`
```

## **Description**

If you have solved WinAntiDbg0x100, you'll discover something new in this one. Debug the executable and find the flag! This challenge executable is a Windows console application, and you can start by running it using Command Prompt on Windows. This executable requires admin privileges. You might want to start Command Prompt or your debugger using the 'Run as administrator' option. Challenge can be downloaded [here](https://artifacts.picoctf.net/c\_titan/58/WinAntiDbg0x200.zip).

<figure><img src="../.gitbook/assets/image (229).png" alt=""><figcaption></figcaption></figure>

Open it with x32dbg.

<figure><img src="../.gitbook/assets/image (237).png" alt=""><figcaption></figcaption></figure>

After analyze it. I set breakpoints as IsDebuggerPresent.

```
bp IsDebuggerPresent
```

<figure><img src="../.gitbook/assets/image (238).png" alt=""><figcaption></figcaption></figure>

Double Click IsDebuggerPresent

<figure><img src="../.gitbook/assets/image (239).png" alt=""><figcaption></figcaption></figure>

## **Solution Strategy**

Click step over, to see how to flow of this app work. And stop at `jne winantidbg0x200.C513E9`

<figure><img src="../.gitbook/assets/image (240).png" alt=""><figcaption></figcaption></figure>

Click step over, to see how to flow of this app work. And stop at `jz winantidbg0x200.C517D3`

<figure><img src="../.gitbook/assets/image (241).png" alt=""><figcaption></figcaption></figure>

All Breakpoints

<figure><img src="../.gitbook/assets/image (243).png" alt=""><figcaption></figcaption></figure>

## Flag

<figure><img src="../.gitbook/assets/image (242).png" alt=""><figcaption></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
