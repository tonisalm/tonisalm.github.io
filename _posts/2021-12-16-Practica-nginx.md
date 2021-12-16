---
typora-copy-images-to: ../assets/images
typora-root-url: ../
title: Práctica nginx
---

# Práctica nginx

**Nginx** (pronunciado en inglés «ényin-ex», /ˈɛndʒɪn-ɛks/) es un [servidor web](https://es.wikipedia.org/wiki/Servidor_web)/[proxy inverso](https://es.wikipedia.org/wiki/Servidor_proxy) ligero de alto rendimiento y un [proxy](https://es.wikipedia.org/wiki/Servidor_proxy) para protocolos de [correo electrónico](https://es.wikipedia.org/wiki/Correo_electrónico) ([IMAP](https://es.wikipedia.org/wiki/Internet_Message_Access_Protocol)/[POP3](https://es.wikipedia.org/wiki/Post_Office_Protocol)).

## Instalación

Para facilitar la instalación de nginx, vamos a utilizar docker, que ya tiene una imagen con todo configurado:

```bash
docker run --rm -d -p 8080:80 --name web nginx
```

Esto descargará la imagen y la iniciará, mapeando el puerto 8080 del host, de modo que si entramos en http://localhost:8080 veremos la web por defecto de nginx:

![image-20211216162757664](/assets/images/image-20211216162757664.png)

## Agregar HTML personalizado

De forma predeterminada, Nginx busca en el directorio `/usr/share/nginx/html` dentro del contenedor los archivos para servir, así que para mostrar una página nuestra lo más sencillo será montar un volumen para vincular un directorio de nuestra máquina local a esa ubicación del contenedor. Creamos un directorio, por ejemplo en `~/Documentos/nginx/web/index.html` y ejecutamos el contenedor de esta manera:

```bash
docker run --rm -d -p 8080:80 --name web -v ~/Documentos/nginx/web:/usr/share/nginx/html nginx
```

Ahora, en lugar de la página por defecto, se mostrará la nuestra:

![image-20211216161715253](/assets/images/image-20211216161715253.png)

## Crear una imagen personalizada

Para crear una imagen personalizada, creamos un Dockerfile en `~/Documentos/nginx` (siguiendo nuestro ejemplo) y agregamos un comando para que copie la web al contenedor cuando lo arranca:

```dockerfile
FROM nginx:latest
COPY ./web /usr/share/nginx/html
```

![image-20211216165457158](/assets/images/image-20211216165457158.png)

Ahora constuimos y ejecutamos nuestra imagen personalizada:

```bash
docker build -t nginx .
docker run --rm -d -p 8080:80 --name web nginx
```

![image-20211216165358540](/assets/images/image-20211216165358540.png)

El resultado es el mismo de antes, pero ahora nuestros archivos locales están separados del contenedor, de manera que si alguien accede al contenedor y los modifica, no estaría modificando la fuente (eso sí, si queremos guardar cambios en algún archivo desde el contenedor, tendríamos que montar el volumen como en el apartado anterior).
