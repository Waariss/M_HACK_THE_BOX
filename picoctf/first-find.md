# First Find

{% code fullWidth="false" %}
```markdown
Tags: `picoGym Exclusive` `General Skills`
```
{% endcode %}

## **Description**

Unzip this archive and find the file named **'uber-secret.txt'**

* [Download zip file](https://artifacts.picoctf.net/c/502/files.zip)

```shell
unzip files.zip
```

<figure><img src="../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>

```sh
find /home/waaris_m/Downloads/files -type f -name "uber-secret.txt"
```

<figure><img src="../.gitbook/assets/image (79).png" alt=""><figcaption></figcaption></figure>

## Flag

```bash
cat /home/waaris_m/Downloads/files/adequate_books/more_books/.secret/deeper_secrets/deepest_secrets/uber-secret.txt
```

<figure><img src="../.gitbook/assets/image (80).png" alt=""><figcaption><p>picoCTF{f1nd_15_f457_ab443fd1}</p></figcaption></figure>

## Follow Me

* **LinkedIn**: [https://www.linkedin.com/in/waris-damkham/](https://www.linkedin.com/in/waris-damkham/)
* **Website**: [https://waris-damkham.netlify.app/](https://waris-damkham.netlify.app/#home)
