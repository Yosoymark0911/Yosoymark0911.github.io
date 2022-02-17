---

typora-copy-images-to: ../assets/img/
typora-root-url: ../
layout: post
categories: 
conToc: true
title: Wordpress
---

# Práctica Wordpress

Para empezar con esta práctica, primero crearemos un directorio y nos moveremos a el:

```bash
mkdir my_wordpress
cd my_wordpress
```

Aquí deberemos crear un archivo llamado docker-compose.yml con el siguiente contenido:

```yaml
version: '3.3'
   
services:
   db:
     image: mysql:5.7
     volumes:
       - db_data:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: somewordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: wordpress
   
   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     ports:
       - "8000:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD: wordpress
       WORDPRESS_DB_NAME: wordpress
volumes:
    db_data: {}
```



Ahora procederemos a contruir el proyecto mediante el comando:

```bash
docker-compose up -d
```

![image-20220107111604482](/assets/img/image-20220107111604482.png)	



![image-20220107111640338](/assets/img/image-20220107111640338.png)
