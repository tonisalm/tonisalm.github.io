---
typora-copy-images-to: ../assets/images/2022-04-26-Seguridad-Docker/
typora-root-url: ../
title: Seguridad Docker
---

# Seguridad en Docker

## Escaneo pasivo de vulnerabilidades

No se debe olvidar que los contenedores son como un sistema cualquiera, están expuestos a las amenazas y si no vamos con cuidado, pueden ser fácilmente atacados. Utilizar regularmente un escáner de seguridad de contenedores, nos ayudará a estar prevenidos.

### Trivy

En esta práctica aprenderemos a usar [trivy](https://aquasecurity.github.io/trivy), que es una utilidad para escanear vulnerabilidades, problemas de configuración y claves desprotegidas dentro de los contenedores.

1. Instala [trivy](https://aquasecurity.github.io/trivy/v0.27.0/getting-started/installation/).
2. Clona el repositorio https://github.com/christophetd/log4shell-vulnerable-app que contiene, entre otras, la vulnerabilidad log4shell.

Ahora entra en el directorio donde has clonado el repositorio. Verás un Dockerfile, escanéalo para detectar problemas de configuración con el siguiente comando:

```bash
trivy config .
```

![image-20220426172202399](/assets/images/2022-04-26-Seguridad-Docker/image-20220426172202399.png)

Como podemos ver, nos indica que la imagen se va a construir con el usuario root por defecto. Esto, en principio, es un problema de seguridad que no es fácil de solucionar, ya que las imágenes no suelen estar pensadas para funcionar con un usuario sin privilegios. [Aquí](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#user) puedes ver más información al respecto, aunque en nuestro caso lo que haremos será ignorar la advertencia.

Genera la imagen y luego escanéala con trivy:

```bash
docker build -t log4shell .
trivy image log4shell
```

![image-20220428160719770](/assets/images/2022-04-26-Seguridad-Docker/image-20220428160719770.png)

Aquí sólo vemos dos de las muchas vulnerabilidades detectadas. Podemos ver diferentes campos que indican la librería afectada, la identificación CVE, el nivel de peligrosidad, etc. Si vamos a la dirección que indica en la última columna, podemos ver más info sobre la vulnerabilidad y cómo corregirla (potencialmente):

![image-20220428160827689](/../../../.config/Typora/typora-user-images/image-20220428160827689.png)

Ahora vamos a ver otro ejemplo. Descargamos una imagen de wordpress de hace unos años y la escaneamos, a ver qué encuentra:

```
docker pull wordpress:4.8.3-php7.0-apache  ## Descarga la imagen
trivy image wordpress:4.8.3-php7.0-apache  ## Escanea la imagen
```

![image-20220428161711127](/assets/images/2022-04-26-Seguridad-Docker/image-20220428161711127.png)

Verás que también sale una cantidad importante de vulnerabilidades, aunque esta imagen no esté pensada para eso como la anterior. En unos años la cosa cambia mucho, por eso es importante estar al día.

### Dependency track

Ahora vamos a escanear la imagen log4shell anterior con [Dependency track](https://tonisalm.github.io/2022/02/22/DependencyTrack.html), otro escáner de vulnerabilidades que ya vimos en una práctica anterior, a partir del BOM generado con [Syft](https://github.com/anchore/syft/):

```bash
syft log4shell -o cyclonedx-xml > log4shell-bom.xml
```

Ahora carga el fichero xml resultante en Dependency track y recarga la página para ver las vulnerabilidades:

![image-20220428165036479](/assets/images/2022-04-26-Seguridad-Docker/image-20220428165036479.png)

### Grype

Por último, escaneamos también con [Grype](https://github.com/anchore/grype/) (del creador de Syft):

![image-20220428165735160](/assets/images/2022-04-26-Seguridad-Docker/image-20220428165735160.png)

Aquí no tenemos una información tan completa como en Dependency track, pero sólo por la enorme cantidad de recursos que consume, vale la pena usar Grype y luego buscarse la vida con los códigos CVE.

