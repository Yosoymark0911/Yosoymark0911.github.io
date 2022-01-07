---

typora-copy-images-to: ../assets/img/
typora-root-url: ../
layout: post
categories: 
conToc: true
title: Nginx
---

# Instalación Nginx

Para empezar esta practica, lo primero que deberemos hacer, es un pull de una imagen Nginx a nuestro ordenador:

```bash
docker pull nginx
```

![image-20220107102426898](/assets/img/image-20220107102426898.png)

Ahora ejecutaremos la imagen con los siguientes parámetros:

```
docker run --rm -d -p 8080:80 --name web nginx
```

![image-20220107102628194](/assets/img/image-20220107102628194.png)

Ahora deberemos parar el contenedor, al que hemos llamado web:

```
docker stop web
```

Lo siguiente que haremos, será que nuestro Docker corra un fichero .HTML, que nosotros le indiquemos, para eso, primero deberemos crear dicho fichero:


```bash
touch /home/user/Documents/site-content/index.html
nano /home/user/Documents/site-content/index.html
# Ahora le añadiremos las siguientes líenas:
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Docker Nginx</title>
</head>
<body>
  <h2>Hello desde Cibeseguridad</h2>
</body>
</html>
```

Ahora volvemos a correr el contenedor, pero indicándole la variable -v para crear un volumen en /usr/share/nginx/html.

```
docker run --rm -d -p 8080:80 --name web -v ~/Documentos/site-content:/usr/share/nginx/html nginx
```

![image-20220107103811254](/assets/img/image-20220107103811254.png)



A continuación deberemos crear un archivo llamado DockerFile dentro de nuestro directorio, y añadirle las siguientes líneas:

```bash
FROM nginx:latest
COPY ./site-content/index.html /usr/share/nginx/html/index.html
```

Y ahora construiremos nuestra imagen a partir de este fichero:

```bash
docker build -t webserver .
```

![image-20220107104306839](/assets/img/image-20220107104306839.png)

Y ahora ejecutaremos Nginx desde la imagen que acabamos de crear:

![image-20220107104434832](/assets/img/image-20220107104434832.png)
