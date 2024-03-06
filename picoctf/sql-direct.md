# SQL Direct

```markdown
Tags: `picoCTF2022` `Web Exploitation` `sql`
```

## **Description**

Connect to this PostgreSQL server and find the flag!

Additional details will be available after launching your challenge instance.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

```sql
SELECT table_schema, table_name
FROM information_schema.tables
WHERE table_type = 'BASE TABLE' AND table_schema NOT IN ('pg_catalog', 'information_schema');
```

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

```sql
SELECT * FROM public.flags;
```

## Flag

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>picoCTF{L3arN_S0m3_5qL_t0d4Y_73b0678f}</p></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
