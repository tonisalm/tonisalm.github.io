---
typora-copy-images-to: ../assets/images
typora-root-url: ../
title: Práctica Wordpress
---

# Práctica Wordpress

Vamos a instalar Wordpress con Docker. Para esto usaremos **docker compose**, que es una herramienta para crear y ejecutar aplicaciones multi-contenedor, donde en un contenedor se ejecutará el servidor web con Wordpress y en otro la base de datos.

En primer lugar creamos un directorio de proyecto, por ejemplo `~/Documentos/wordpress`, y dentro un archivo **docker-compose.yml** que define lo que se va a ejecutar:

![image-20211216171110906](/assets/images/image-20211216171110906.png)

En la imagen vemos que se va a ejecutar un contenedor para la base de datos "db", con una imagen de mysql, que guardará los datos en un directorio local "db_data", y que se reiniciará automáticamente si cae. Tambien crea una serie de variables de entorno necesarias para la ejecución de la base de datos: nombre de la base de datos, contraseña de root, usuario y contraseña de usuario de la base de datos.

Por otro lado va a crear otro contenedor "wordpress" que depende de "db", es decir, que hasta que no arranque "db" no iniciará "wordpress". Fíjate que le pasamos también una serie de variables que enlazan con el contenedor de la base de datos.

**NOTA**: El archivo de la imagen no funcionaría porque la primera línea indica la versión del archivo *compose* y no puede ser "0.1". Según la versión de Docker que utilicemos, podremos usar una serie de versiones de *compose*. En nuestro ejemplo podríamos poner "3.3". Para saber más sobre este tema, visita:

[https://docs.docker.com/compose/compose-file/compose-versioning/](https://docs.docker.com/compose/compose-file/compose-versioning/)

Por último, sólo nos quedará construir el proyecto y ejecutarlo (desde el directorio de nuestro proyecto):

```bash
docker-compose up -d
```

![image-20211216174500941](/assets/images/image-20211216174500941.png)

La primera vez tardará un poco. Una vez finalizado el proceso, abrimos el navegador con la dirección `localhost:8000` (el puerto que le hemos indicado en el archivo compose) y ya entraremos en nuestro Wordpress:

![image-20211216174726504](/assets/images/image-20211216174726504.png)
