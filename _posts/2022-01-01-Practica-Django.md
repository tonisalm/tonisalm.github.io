---
typora-copy-images-to: ../assets/images
typora-root-url: ../
title: Práctica Django
---

# Práctica Django

Siguiendo las prácticas con **docker compose**, vamos a instalar el framework de desarrollo web Django. Al igual que en la práctica anterior, en primer lugar crea un directorio de proyecto desde donde realizarás todas las acciones.

## Crear un proyecto Django

En primer lugar crea un nuevo archivo llamado `Dockerfile` con el siguiente contenido:

```dockerfile
# syntax=docker/dockerfile:1
FROM python:3
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
WORKDIR /code
COPY requirements.txt /code/
RUN pip install -r requirements.txt
COPY . /code/
```

Y un archivo de texto llamado `requirements.txt` con el siguiente contenido:

```
Django>=3.0,<4.0
psycopg2>=2.8
```

Ahora ya tenemos lo necesario para construir nuestro servicio web a partir de la imagen python donde instalará Django y psycopg2, un conector de base de datos postgreSQL para python. El siguiente paso será crear un archivo llamado `docker-compose.yml`, que describe los servicios que tendrá nuestra app:

```yaml
services:
  db:
    image: postgres
    environment:
      - POSTGRES_NAME=postgres
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
    volumes:
      - ./data/db:/var/lib/postgresql/data
  web:
    build: .
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/code
    ports:
      - "8000:8000"
    environment:
      - POSTGRES_NAME=postgres
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_HOST_AUTH_METHOD=trust
    depends_on:
      - db
```

Por un lado tenemos el servicio "db" para la base de datos postgreSQL y por otro el servicio "web", que contiene Django. El siguiente paso será crear el proyecto. Ejecuta el siguiente comando:

```bash
 sudo docker-compose run web django-admin startproject composeexample .
```

Esto ejecutará "web", que depende de "db", así que descargará la imagen "postgres", que guardará la base de datos en nuestra carpeta de proyecto, construirá la imagen "web" a partir del fichero Dockerfile que hemos creado antes. Por último, ejecutará el comando `django-admin`, que iniciará el proyecto "composeexample" (puedes poner el nombre que quieras) en el directorio actual:

![image-20211231195655922](/assets/images/image-20211231195655922.png)

En el directorio de trabajo nos quedarán estos archivos:

![image-20220101171828465](/assets/images/image-20220101171828465.png)

Es posible que no puedas acceder a ellos, ya que son propiedad de root. Lo solucionamos con:

```bash
sudo chown -R $USER:$USER .
```

***NOTA:** Si, como es mi caso, trabajas con carpetas compartidas en VirtualBox y tu carpeta de proyecto está físicamente en un volumen NTFS, verás que aunque intentes cambiar permisos se va a quedar igual. En este caso, copia la carpeta del proyecto a otro lugar dentro del sistema linux y verifica los permisos o no funcionará lo que viene a continuación.*

## Conectar la base de datos

Entra en la carpeta "composeexample" (o el nombre que le hayas puesto) y edita el fichero `settings.py`. Reemplaza el contenido de la sección DATABASES para que quede así (no olvides el "import os"):

```python
import os
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': os.environ.get('POSTGRES_NAME'),
        'USER': os.environ.get('POSTGRES_USER'),
        'PASSWORD': os.environ.get('POSTGRES_PASSWORD'),
        'HOST': 'db',
        'PORT': 5432,
    }
}
```

Estos parámetros vienen dados por la configuración por defecto de la imagen postgres. Si los cambias, deberías ajustarlos a tu configuración. A continuación, ejecuta el comando que inicia todo:

```bash
 docker-compose up
```

![image-20220101175037494](/assets/images/image-20220101175037494.png)

Ahora ve a `localhost:8000` en tu navegador y verás la página de bienvenida de Django:

![image-20220101175319886](/assets/images/image-20220101175319886.png)

Para cerrarlo, simplemente vuelve al terminal y pulsa `Ctrl+C`, o bien ejecuta el comando `docker-compose down` desde el directorio del proyecto.
