---

typora-copy-images-to: ../assets/img/
typora-root-url: ../
layout: post
categories: 
conToc: true
title: Dependecy Track
---

# Práctica Dependecy Track

Para esta practica, la MV, deberá tener más de 4GB de RAM para que funcione este sistema, si tiene menos o solo 4GB no funcionará.

Para empezar, descargaremos y montaremos el docker con los siguientes comandos:

```
# Downloads the latest Docker Compose file
curl -LO https://dependencytrack.org/docker-compose.yml

# Starts the stack using Docker Compose
docker-compose up -d
```

Una vez dentro, nos pedirá iniciar sesión:

![image-20220405181205290](/assets/img/image-20220405181205290.png)

La primera vez que iniciemos sesión, el usuario y la contraseña serán admin/admin, luego nos pedirá cambiar tanto el usuario como la contraseña.

Una vez dentro, deberemos descargarnos lo siguiente:

```
https://github.com/CycloneDX/bom-examples/tree/master/SBOM
```

Una vez descargado, volveremos al programa, crearemos un nueovo poyecto y subiremos uno de los archivos de vulnerabilidades que se nos ofrece, una vez subido, recargamos la página y deberíamos ver algo como esto:

![image-20220405181947130](/assets/img/image-20220405181947130.png)
