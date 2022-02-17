---
typora-copy-images-to: ../assets/img/
typora-root-url: ../
layout: post
categories: 
conToc: true
title: DockerHub
---

# Primer Docker Hub

Para empezar con la practica, deberemos tener una cuenta en Docker, en caso de que no la tengamos la podremos crear en el siguiente enlace:

https://hub.docker.com/

En el momento que tengamos una cuenta creada iniciaremos sesión desde la línea de comandos:

![image-20220107101646240](/assets/img/image-20220107101646240.png)



Ahora prepararemos nuestra imagen para que sea aceptada por Docker Hub:

```bash
docker tag chapter2 mulldemolins/chapter2:latest
```

![image-20220107101754056](/assets/img/image-20220107101754056.png)

Y ahora la subiremos a Docker Hub mediante el comando 

```
docker push mulldemolins/chapter2:latest
```

![image-20220107101930217](/assets/img/image-20220107101930217.png)

Ahora comprobaremos que la imagen se ha subido de manera correcta:

![image-20220107102113119](/assets/img/image-20220107102113119.png)
