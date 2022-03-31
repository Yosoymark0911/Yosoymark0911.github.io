---

typora-copy-images-to: ../assets/img/
typora-root-url: ../
layout: post
categories: 
conToc: true
title: Heroku
---

# Práctica Heroku

Antes de nada, deberemos crearnos una cuenta en la página web de [Heroku](https://id.heroku.com/login), una vez con la cuenta creada, pasaremos al punto de la instalación del paquete.

En un principio en las practicas se nos aconseja que usemos el siguiente comando: 

```bash
sudo snap install heroku --classic
```

Pero este a mi me ha dado problemas, por ello, buscando información en la página oficial he visto este otro comando que a mi me ha funcionado:

```bash
curl https://cli-assets.heroku.com/install-ubuntu.sh | sh
```

Una vez ejecutado, ya lo tendremos instalado, ahora simplemente iniciamos sesión con 

```bash
heroku login
```

Y esto nos debería abrir un navegador, aquí iniciaremos sesión

![image-20220331160803582](/assets/img/image-20220331160803582.png)



Ahora, empezaremos con la preparación de la instalación, para ello, primero nos descargaremos el siguiente repositorio de [GitHub](https://github.com/heroku/python-getting-started.git) y nos moveremos dentro del repositorio descargado.

Una vez lo hayamos descargado y estemos dentro ejecutaremos el siguiente comando: 

```bash
heroku create
```

Y finalmente desplegaremos el código con:

```bash
git push heroku main
```

Para comprobar que ha ido todo bien, podemos hacer un:

```bash
heroku ps
```

Y podemos ver, que tenemos una máquina montada:

![image-20220331161932045](/assets/img/image-20220331161932045.png)

Lo siguiente que haremos será la instalación de todas las dependencias de la aplicación ya que sin estas no podremos ejecutar la app. Para ello ejecutaremos los siguientes comandos:

Primero, deberemos ir al paquete  requirements.txt y añadir la siguiente línea:

```
django
gunicorn
django-heroku
requests
```

Y ahora instalaremos las dependencias de la aplicación, para ello deberemos ejecutar los siguientes comandos:

```
sudo apt-get install python3.9-venv			#Instalación del entorno virtual de Python
python3 -m venv venv						# Creación de este espacio virtual
source venv/bin/activate					# Activar este entorno
sudo apt install python3-pip				#Instalar de un paquete necesario
sudo apt-get install libpq-dev				#Instalar de un paquete necesario
pip install -r requirements.txt			#Instalar los requisitos que se nos ha indicado
```

Y finalmente, modificaremos el fichero hello/views.py de la siguiente manera:

```python
def index(request):
    r = requests.get('https://httpbin.org/status/418')
    return HttpResponse('<pre>' + r.text + '</pre>')
```

Y ahora ejecutaremos el programa y nos debería aparecer lo siguiente:

![image-20220331164134795](/assets/img/image-20220331164134795.png)
