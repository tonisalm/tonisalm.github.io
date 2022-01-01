---
typora-copy-images-to: ../assets/images/2022-01-01-Practica-Rails
typora-root-url: ../
title: Práctica Rails
---

# Práctica Rails

Siguiendo las prácticas con **docker compose**, vamos a instalar el framework de desarrollo web Rails. Al igual que en la práctica anterior, en primer lugar crea un directorio de proyecto desde donde realizarás todas las acciones.

## Definir el proyecto

En primer lugar crea un nuevo archivo llamado `Dockerfile` con el siguiente contenido:

```dockerfile
# syntax=docker/dockerfile:1
FROM ruby:2.5
RUN apt-get update -qq && apt-get install -y nodejs postgresql-client
WORKDIR /myapp
COPY Gemfile /myapp/Gemfile
COPY Gemfile.lock /myapp/Gemfile.lock
RUN bundle install

# Add a script to be executed every time the container starts.
COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
EXPOSE 3000

# Configure the main process to run when running the image
CMD ["rails", "server", "-b", "0.0.0.0"]
```

Esto construirá un contenedor con Ruby on Rails y todas las dependencias necesarias. Si quieres saber más sobre cómo funcionan los Dockerfiles, lee la [guía de usuario de Docker](https://docs.docker.com/get-started/) y la [referencia de archivos Dockerfile](https://docs.docker.com/engine/reference/builder/).

Ahora crea un archivo de texto llamado `Gemfile` con el siguiente contenido:

```
source 'https://rubygems.org'
gem 'rails', '~>5'
```

Un archivo `Gemfile.lock` vacío:

```bash
touch Gemfile.lock
```

Un archivo `entrypoint.sh` con el siguiente contenido, que evita un error que impide iniciar el servidor:

```sh
#!/bin/bash
set -e

# Remove a potentially pre-existing server.pid for Rails.
rm -f /myapp/tmp/pids/server.pid

# Then exec the container's main process (what's set as CMD in the Dockerfile).
exec "$@"
```

Y finalmente un archivo `docker-compose.yaml`, que describe los servicios que forman la app, en este caso una base de datos (db) postgreSQL y un servidor web (web) con Rails:

```yaml
services:
  db:
    image: postgres
    volumes:
      - ./tmp/db:/var/lib/postgresql/data
    environment:
      POSTGRES_PASSWORD: password
  web:
    build: .
    command: bash -c "rm -f tmp/pids/server.pid && bundle exec rails s -p 3000 -b '0.0.0.0'"
    volumes:
      - .:/myapp
    ports:
      - "3000:3000"
    depends_on:
      - db
```

## Construir el proyecto

Ejecuta el siguiente comando para generar el esqueleto de la aplicación:

```bash
docker-compose run --no-deps web rails new . --force --database=postgresql
```

Esto ejecutará "web", que depende de "db", así que descargará la imagen "postgres", que guardará la base de datos en nuestra carpeta de proyecto, construirá la imagen "web" a partir del fichero Dockerfile que hemos creado antes y gracias al parámetro `--no-deps` no iniciará servicios vinculados. Por último, ejecutará el comando `rails new`, que iniciará el nuevo proyecto en el directorio actual:

![image-20220101190545125](/assets/images/2022-01-01-Practica-Rails/image-20220101190545125.png)

Al finalizar, en el directorio de trabajo quedarán estos archivos:

![image-20220101191702082](/assets/images/2022-01-01-Practica-Rails/image-20220101191702082.png)

Al ejecutar Docker en Linux, los archivos generados por el comando `rails new` son propiedad de root. Cambiamos la propiedad a nuestro usuario, o de lo contrario fallará más adelante:

```bash
sudo chown -R $USER:$USER .
```

Y por último, construimos la imagen. Este paso hay que repetirlo siempre que hagamos un cambio en `Gemfile`:

```bash
docker-compose build
```

## Conectar la base de datos

Ahora hay que configurar Rails para que utilice la base de datos del contenedor que hemos preparado. Edita el fichero `config/database.yml` y reemplaza su contenido por el siguiente:

```yaml
default: &default
  adapter: postgresql
  encoding: unicode
  host: db
  username: postgres
  password: password
  pool: 5

development:
  <<: *default
  database: myapp_development

test:
  <<: *default
  database: myapp_test
```

A continuación, inicia la aplicación:

```bash
docker-compose up
```

![image-20220101193727800](/assets/images/2022-01-01-Practica-Rails/image-20220101193727800.png)

Ahora, en otro terminal, ejecuta el siguiente comando para crear la base de datos:

```bash
docker-compose run web rake db:create
```

![image-20220101194000684](/assets/images/2022-01-01-Practica-Rails/image-20220101194000684.png)

Y ya está. Ahora ve a `localhost:3000` en tu navegador y verás la página de bienvenida de Rails:

![image-20220101194212161](/assets/images/2022-01-01-Practica-Rails/image-20220101194212161.png)

Para cerrarlo, ejecuta el comando `docker-compose down` desde el directorio del proyecto. Y para iniciarlo de nuevo, usa `docker-compose up`.
