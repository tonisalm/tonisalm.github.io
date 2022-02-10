---
typora-copy-images-to: ../assets/images/2022-01-13-Practica-Aplicacion-en-Python/
typora-root-url: ../
title: Práctica Aplicación en Python
---

# Práctica Aplicación en Python

En esta práctica crearemos una aplicación en Python con docker-compose y usaremos tags y branches en Git.

## Creando la imagen docker

Crea un nuevo repositorio para la aplicación en GitHub, clónalo en local y crea dentro esta estructura y contenido:

![image-20220118160128375](/assets/images/2022-01-13-Practica-Aplicacion-en-Python/image-20220118160128375.png)

Contenido de `Dockerfile`:

```dockerfile
FROM python:3.4
RUN pip install Flask==0.10.1
WORKDIR /app
COPY app /app
CMD ["python", "identidock.py"]
```

Contenido de `identidock.py`:

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello World from Ciberseguridad!\n'

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')
```

Ahora, desde la carpeta del proyecto generas la imagen y la pones en marcha. La carpeta "app" la montamos en un volumen persistente para no tener que estar reiniciando el contenedor cada vez que hagamos algún cambio en la aplicación:

```bash
docker build -t identidock .
docker run -d -p 5000:5000 -v "$(pwd)"/app:/app identidock
```

Ahora prueba si funciona. Tiene que devolver el texto que hemos puesto en el archivo `identidock.py`:

```bash
curl localhost:5000
```

![image-20220118161541810](/assets/images/2022-01-13-Practica-Aplicacion-en-Python/image-20220118161541810.png)

## Creando un tag

Ya tienes la versión 1 de tu flamante aplicación. Ahora hay que actualizar el repositorio y guardarla debidamente, creando un *tag*:

```bash
git add *
git commit -m "Aplicación chanante v1"
git push
git tag v1
git push origin v1
```

Perfecto. ¡Ahora a por la versión 2! Si la liamos parda, siempre nos quedará la versión 1.

## Creando una rama (*branching*)

Ahora imagina que vas a trabajar en tu aplicación junto con más gente y que cada uno se encargará de hacer algo en particular. Git ofrece un sistema de *branching* para que cada uno pueda trabajar en su rama sin afectar al resto y, una vez está todo ok, se fusiona (*merge*) el contenido de la rama con la rama principal (*master* o *main*) y se sigue trabajando.

Crea una nueva rama llamada "identicons" mediante el comando:

```bash
git branch -d identicons
```

Aquí usamos el parámetro `-d` para crear la rama y además movernos a ella, indicando a Git que los cambios que hagamos a partir de ahora se ubicarán en esta rama. Si estuviésemos haciendo cosas en diferentes ramas, nos moveríamos de una a otra con el comando `git checkout `*`nombre-de-rama`*. Cada rama guarda el estado de sus archivos, de modo que no perderíamos nada. Más adelante, cuando acabes el trabajo, veremos cómo fusionar ramas.

## Aplicación v2

Hay que reconocer que la flamante aplicación de momento no hace gran cosa, pero vamos a darle caña. El objetivo de la v2 será mostrar un formulario donde el usuario escribe su nombre y al pulsar el botón de enviar, la aplicación genera un avatar para ese usuario y lo muestra en pantalla.

![image-20220210164115326](/assets/images/2022-01-13-Practica-Aplicacion-en-Python/image-20220210164115326.png)

Para conseguirlo, crea en el directorio raíz del proyecto un archivo `docker-compose.yml` que añada un contenedor `dnmonster` a la aplicación, que proporcionará avatares únicos para una cadena de texto dada:

```yaml
identidock:
  build: .
  ports:
    - "5000:5000"
  volumes:
    - ./app:/app
  links:
    - dnmonster
dnmonster:
  ports:
    - "5080:8080"
  image: amouat/dnmonster:1.0
```

Después cambia el archivo `Dockerfile` para que instale el paquete `requests`, necesario para obtener datos desde `dnmonster`:

```dockerfile
FROM python:3.4
RUN pip install Flask==0.10.1 requests==2.5.1
WORKDIR /app
CMD ["python", "identidock.py"]
```

Ahora cambia el script `identidock.py`para que muestre el formulario, que enviará a `dnmonster` el nombre que introduzcas como parámetro por `POST y luego usará el método `get_identicon` para obtener la imagen del avatar. No olvides añadir un poco de sal propia para que tus avatares no sean iguales a los de otra aplicación y recuerda que python es sensible a la indentación, así que no mezcles espacios con tabuladores o te dará errores:

```python
from flask import Flask, Response, request
import requests
import hashlib
app = Flask(__name__)
salt = "ESTA-ES-MI-SAL"
default_name = 'tu nombre aquí'

@app.route('/', methods=['GET', 'POST'])
def mainpage():
	name = default_name
	if request.method == 'POST':
		name = request.form['name']
	salted_name = salt + name
	name_hash = hashlib.sha256(salted_name.encode()).hexdigest()
	header = '<html><head><title>Identidock</title></head><body>'
	body = '''<form method="POST">
			Hola <input type="text" name="name" value="{0}">
			<p><input type="submit" value="¡Crea tu avatar!"></p>
			</form>
			<p>Este es tu avatar:</p>
			<img src="/monster/{1}"/>
			'''.format(name, name_hash)
	footer = '</body></html>'
	return header + body + footer
@app.route('/monster/<name>')
def get_identicon(name):
	r = requests.get('http://dnmonster:8080/monster/' + name + '?size=80')
	image = r.content
	return Response(image, mimetype='image/png')

if __name__ == '__main__':
	app.run(debug=True, host='0.0.0.0')
```

Ahora móntalo todo con el comando `docker-compose up -d` y comprueba que cada vez que pones un nombre, devuelve un avatar distinto (pero siempre el mismo para el mismo nombre):

![image-20220210164115326](/assets/images/2022-01-13-Practica-Aplicacion-en-Python/image-20220210164115326.png)

![image-20220210164228170](/assets/images/2022-01-13-Practica-Aplicacion-en-Python/image-20220210164228170.png)

## Fusionando ramas

Ahora es el momento de fusionar la rama `identicons` con la rama principal. Para ello:

```bash
git add *
git commit -m "Finalizar identicons"
git checkout main
```

Si revisas ahora el código, verás que está como estaba  antes de empezar la rama `identicons`. Ahora fusiónalas:

```bash
git merge identicons 
```

![image-20220210165016443](/assets/images/2022-01-13-Practica-Aplicacion-en-Python/image-20220210165016443.png)

Y ya tenemos aplicados los cambios. Ahora elimina la rama `identicons`, sube los cambios y crea un nuevo tag:

```bash
git branch -d identicons
git push
git tag v2
git push origin v2
```

## Añadiendo cacheo

Por crear uno o dos monstruos no se va a morir tu ordenador, pero imagina que tienes cientos o miles de peticiones. Para evitar carga de trabajo, podemos usar **caché** para que no vuelva a calcular los monstruos que ya ha calculado. Usaremos `Redis`, que es un almacén de datos de clave-valor en memoria. Vamos a añadir un nuevo contenedor con la imagen oficial de Redis. Modifica `docker-compose.yml`para añadirlo:

```yaml
identidock:
  build: .
  ports:
    - "5000:5000"
  volumes:
    - ./app:/app
  links:
    - dnmonster
    - redis
dnmonster:
  ports:
    - "5080:8080"
  image: amouat/dnmonster:1.0
redis:
  image: redis:3.0
```

Ahora instala Redis dentro de la imagen del proyecto. Modifica esta línea en `Dockerfile`:

```dockerfile
RUN pip install Flask==0.10.1 requests==2.5.1 redis==2.10.3
```

Y modifica el script `identidock.py` para que utilice la caché:

```python
from flask import Flask, Response, request
import requests
import hashlib
import redis

app = Flask(__name__)
cache = redis.StrictRedis(host = 'redis', port = 6379, db = 0)
salt = "ESTA-ES-MI-SAL"
default_name = 'tu nombre aquí'

@app.route('/', methods=['GET', 'POST'])
def mainpage():
	name = default_name
	if request.method == 'POST':
		name = request.form['name']
	salted_name = salt + name
	name_hash = hashlib.sha256(salted_name.encode()).hexdigest()
	header = '<html><head><title>Identidock</title></head><body>'
	body = '''<form method="POST">
			Hola <input type="text" name="name" value="{0}">
			<p><input type="submit" value="¡Crea tu avatar!"></p>
			</form>
			<p>Este es tu avatar:</p>
			<img src="/monster/{1}"/>
			'''.format(name, name_hash)
	footer = '</body></html>'
	return header + body + footer

@app.route('/monster/<name>')
def get_identicon(name):
	image = cache.get(name)
	if image is None:
		print ("Cache miss", flush=True)
		r = requests.get('http://dnmonster:8080/monster/' + name + '?size=80')
		image = r.content
		cache.set(name, image)
	return Response(image, mimetype='image/png')

if __name__ == '__main__':
	app.run(debug=True, host='0.0.0.0')
```

Ahora sólo hay que parar el contenedor, construirlo y levantarlo:

```bash
docker-compose stop
docker-compose build
docker-compose up -d
```

Con `docker logs ref_identidock` podemos ver si la caché funciona (dice "Cache miss" cuando no encuentra el monstruo en la caché y tiene que generarlo):

![image-20220210172527286](/assets/images/2022-01-13-Practica-Aplicacion-en-Python/image-20220210172527286.png)

¡Perfecto! Ahora no olvides subir los cambios con el tag `v3`
