---
typora-copy-images-to: ../assets/images/2022-03-10-DockerHub-Imagen-Privada/
typora-root-url: ../
title: DockerHub Imagen Privada
---

# DockerHub Imagen Privada

Vamos a crear una imagen privada en el registro de Docker para desplegarla mediante K8s:

1. [Crea una imagen personalizada de nginx](https://tonisalm.github.io/2021/12/16/Practica-nginx.html)

2. [Sube la imagen a DockerHub](https://tonisalm.github.io/2021/12/09/Docker-Hub.html)

3. Entra en DockerHub y haz la imagen privada:
   ![image-20220310174526856](/assets/images/2022-03-10-DockerHub-Imagen-Privada/image-20220310174526856.png)
   
4. Si no lo has hecho aún, inicia K8:
   ![image-20220315154021970](/assets/images/2022-03-10-DockerHub-Imagen-Privada/image-20220315154021970.png)

5. Crea un secreto de tipo docker-registry. En username, password y email, pon los datos de acceso a tu DockerHub. "regtoni" es el nombre que hemos dado al secreto, para luego referenciarlo:
   ![image-20220315154430455](/assets/images/2022-03-10-DockerHub-Imagen-Privada/image-20220315154430455.png)
   ![image-20220315155138799](/assets/images/2022-03-10-DockerHub-Imagen-Privada/image-20220315155138799.png)

6. Crea un archivo de configuración .yaml para desplegar la aplicación en kubernetes. Ten en cuenta la ruta de tu imagen, la versión (si la has puesto) y el nombre del secreto que has utilizado. Una vez lo tengas, inicia el despliegue con `kubectl apply -f .` (si estás en el directorio donde has creado el .yaml):

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: hello1
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
           image: tonisalm/hello1:v1
           ports:
           - containerPort: 80
         imagePullSecrets:
         - name: regtoni
   ```
   
7. Haz un port-forward y entra en el navegador en `localhost:8081` para ver si la aplicación funciona:
   ![image-20220315161744028](/assets/images/2022-03-10-DockerHub-Imagen-Privada/image-20220315161744028.png)

   ![image-20220315162133093](/assets/images/2022-03-10-DockerHub-Imagen-Privada/image-20211216161715253.png)

8. Si no funcionase, comprueba si los pods tienen algún problema mirando el log:
   ![image-20220315162133093](/assets/images/2022-03-10-DockerHub-Imagen-Privada/image-20220315162133093.png)