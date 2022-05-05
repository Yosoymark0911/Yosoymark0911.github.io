---

typora-copy-images-to: ../assets/img/
typora-root-url: ../
layout: post
categories: 
conToc: true
title: DockerHub Imagen Privada
---

# Práctica DockerHub Imagen Privada

Para esta práctica primero deberemos haber hechos tanto [Nginx](https://yosoymark0911.github.io/2021/11/18/Nginx.html) como [DockerHub](https://yosoymark0911.github.io/2021/11/18/Nginx.html), si tenemos estas dos practicas hechas, podremos proceder hacer esta.

Empezaremos convirtiendo la imagen subido a DockerHub en una imagen privada:

 ![image-20220424215325146](/assets/img/image-20220424215325146.png)

Ahora, procederemos a instalar minikube, en caso de que no lo tengamos:

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
sudo dpkg -i minikube_latest_amd64.deb
minikube start
```

![image-20220424215427294](/assets/img/image-20220424215427294.png)

Una vez con esto, procederemos a crear un docker-registry donde tendremos que indicarle un secreto, nuestro correo, nombre de usuario y  nuestra contraseña, para que Kubernetes pueda tener acceso a la imagen privada:

```
kubectl create secret docker-registry hola01 --docker-server=https/index.docker.io/v1 --docker-username={username} --docker-password={password} --docker-email={correo}
```

![image-20220424220453298](/assets/img/image-20220424220453298.png)

Ahora crearemos el fichero .yaml:

```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: chapter2
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: PPP/chapter2:v1
        ports:
        - containerPort: 80
      imagePullSecrets:
      - name: hola01
```

Y ahora crearemos la imagen: 

```bash
kubectl apply -f .
```

![image-20220424220901766](/assets/img/image-20220424220901766.png)

Y ahora si entrado en localhost:8080 debería aparecer nuestra imagen subida de nginx:

![image-20220424221453467](/assets/img/image-20220424221453467.png)

