---
typora-copy-images-to: ../assets/images
typora-root-url: ../
title: Docker Hub
---

# Docker Hub

En esta práctica vamos a publicar nuestra imagen en Docker Hub, de manera que otros puedan descargarla y probarla.

1. Registrarte en [Docker Hub](https://hub.docker.com)

2. Iniciar sesión:

   `docker login`
   ![image-20211214161812487](/assets/images/image-20211214161812487.png)

3. Preparar la imagen:

   `docker tag nombreimagen usuario/nombreimagen:version`
   ![image-20211214163634088](/assets/images/image-20211214163634088.png)

4. Subir la imagen:

   `docker push usuario/imagen:version`
   ![image-20211214163855908](/assets/images/image-20211214163855908.png)

5. Comprobar que está en [Docker Hub](https://hub.docker.com/r/tonisalm/chapter2)

   ![image-20211214164222818](/assets/images/image-20211214164222818.png)

   

