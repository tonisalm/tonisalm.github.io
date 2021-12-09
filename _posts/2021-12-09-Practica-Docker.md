---
typora-copy-images-to: ../assets/images
typora-root-url: ../
title: Práctica Docker
---

# Práctica Docker

## Requisitos previos

Para realizar esta práctica debemos tener instalado Docker y vamos a usar un repositorio de GitHub que ya tiene todos los componentes contenedorizados. Se encuentra en https://github.com/victorponz/docker/tree/master/P1 y se trata de una pequeña aplicación Apache+PHP con una serie de archivos `sh` para ahorrarnos el trabajo de escribir comandos.

## Creación del contenedor

![image-20211209165056728](/assets/images/image-20211209165056728.png)

Una vez descargado el repositorio, al entrar en el directorio y ejecutar `build.sh`, construye el contenedor llamado "chapter2" según lo descrito en el fichero `Dockerfile` que se encuentra en el mismo directorio:

![image-20211209165242781](/assets/images/image-20211209165242781.png)

Lo que hace es descargar de DockerHub la imagen "debian" y establecer una serie de parámetros para crear los directorios de trabajo, instalar las herramientas deseadas y ejecutar `entrypoint.sh`, que ejecuta Apache en modo *foreground* para que no se cierre automáticamente:

![image-20211209170455312](/assets/images/image-20211209170455312.png)



## Ejecutar el contenedor

**Debug.sh:**

![image-20211209165922088](/assets/images/image-20211209165922088.png)

Este script lo que hace es ejecutar nuestro contenedor "chapter2", en modo no persistente (--rm), asignando el puerto 80 del contenedor al 8086 del host, y montando el directorio actual del host en `/home/app` del contenedor, de modo que podamos ver el output de apache en la consola.

![image-20211209170338612](/assets/images/image-20211209170338612.png)

Cuando accedemos (fuera del contenedor) a la dirección que nos indica, vemos la página que hay dentro de `public_html`:

![image-20211209170707123](/assets/images/image-20211209170707123.png)

## Modo persistente

La web que nos muestra tiene un contador que almacena en un archivo llamado `counter.txt`, que sólo está en este contenedor. Si paramos el contenedor y ejecutamos otro, perderemos el contador. Para evitarlo, en lugar de ejecutar `debug.sh`, ejecutaremos `persist.sh`:

![image-20211209170946130](/assets/images/image-20211209170946130.png)

Lo que hace este script es lo mismo que el anterior, pero montando un volumen persistente llamado `/data`, que se crea fuera del contenedor en `/var/lib/docker/volumes/name/_data`.

Cada vez que ejecutemos el script creará un contenedor nuevo (recuerda que con --rm no se guardaba nada), pero montará el mismo volumen persistente, que nuestra web utilizará para crear el contador (porque está así programada), de modo que no perderemos el contador.

