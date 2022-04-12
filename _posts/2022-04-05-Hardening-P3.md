---
typora-copy-images-to: ../assets/images/2022-04-05-Hardening-P2/
typora-root-url: ../
title: Hardening Práctica 3
---

# Hardening Práctica 3

En esta práctica vamos a instalar un firewall de aplicaciones web llamado [Modsecurity](https://es.wikipedia.org/wiki/Mod_Security), al que instalaremos las reglas OWASP, que incluyen de serie una protección bastante buena. Para este propósito, vamos a reutilizar la imagen docker "victima" que creamos en la [práctica anterior](https://tonisalm.github.io/2022/03/15/Validacion.html).

En primer lugar hay que modificar el `Dockerfile` para que haga lo siguiente:

* Añadir la instalación de git (`git`) y modsecurity (`libapache2-mod-security2`)
* Descargar y copiar las reglas OWASP
* Copiar los archivos de configuración `security2.conf` y `000-default.conf` (que prepararemos después) para que apache use las reglas.
* Añadir algunos "tweaks" de seguridad, que desactivan el módulo *autoindex* y la signatura.

```dockerfile
# we will inherit from  the Debian image on DockerHub
FROM debian

# set timezone so files' timestamps are correct
ENV TZ=Europe/Madrid

# install apache and php 7.4
# we include procps and telnet so you can use these with shell.sh prompt
RUN apt-get update -qq >/dev/null && apt-get install -y -qq procps telnet apache2 git libapache2-mod-security2 php7.4 -qq >/dev/null

#Descarga y copia las reglas
WORKDIR /tmp/
RUN git clone https://github.com/SpiderLabs/owasp-modsecurity-crs.git
WORKDIR /tmp/owasp-modsecurity-crs/
RUN cp crs-setup.conf.example /etc/modsecurity/crs-setup.conf
RUN cp -r rules/ /etc/modsecurity
COPY security2.conf /etc/apache2/mods-enabled/security2.conf
COPY 000-default.conf /etc/apache2/sites-available/000-default.conf

#Tweaks de seguridad
RUN echo "ServerTokens ProductOnly" >> /etc/apache2/apache2.conf
RUN echo "ServerSignature Off" >> /etc/apache2/apache2.conf
RUN a2dismod -f autoindex.load

# HTML server directory
WORKDIR /var/www/html
COPY . /var/www/html/

# The PHP app is going to save its state in /data so we make a /data inside the container
RUN mkdir /data && chown -R www-data /data && chmod 755 /data & chmod 755 -R /var/www/html/

# we need custom php configuration file to enable userdirs
COPY php.conf /etc/apache2/mods-available/php7.4.conf

# enable userdir and php
RUN a2enmod php7.4

# we run a script to stat the server; the array syntax makes it so will work as we want
CMD  ["./entrypoint.sh"]

```

Ahora crea el archivo `security2.conf` con el siguiente contenido:

```xml
<IfModule security2_module>
	# Default Debian dir for modsecurity's persistent data
	SecDataDir /var/cache/modsecurity
	SecRuleEngine On
	# Keeping your local configuration in that directory
	# will allow for an easy upgrade of THIS file and
	# make your life easier
    IncludeOptional /etc/modsecurity/*.conf
	Include /etc/modsecurity/rules/*.conf	
</IfModule>
```

Y también el archivo `000-default.conf`:

```
<VirtualHost *:80>

	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/html
	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined

	# Reglas de seguridad
	SecRuleEngine On
	SecRule ARGS:testparam "@contains test" "id:1234,deny,status:403,msg:'Cazado por Ciberseguridad'"

</VirtualHost>

```

Construimos y ejecutamos la imagen:

```bash
docker build -t apachehard .
docker run --detach --rm -p8080:80 -v `pwd`:/data --name="apachehard" apachehard
```

Y hacemos pruebas con diferentes ataques para ver si funciona todo correctamente:

![image-20220405180018740](/assets/images/2022-04-05-Hardening-P2/image-20220405180018740.png)

Ataque *command injection*:

![image-20220405182028233](/assets/images/2022-04-05-Hardening-P2/image-20220405182028233.png)

Ataque *path traversal*:

![image-20220405183052081](/assets/images/2022-04-05-Hardening-P2/image-20220405183052081.png)

Comprobamos el log de apache para ver que efectivamente han sido bloqueados por modsecurity (`docker exec -t -i apachehard /bin/bash` y entramos en `/var/log/apache2/error.log`):

![image-20220405183945210](/assets/images/2022-04-05-Hardening-P2/image-20220405183945210.png)
