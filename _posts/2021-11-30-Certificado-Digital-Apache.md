---
typora-copy-images-to: ../assets/images
typora-root-url: ../
title: Certificado Digital en Apache
---

# Certificado Digital en Apache

Si te interesa activar HTTPS en Apache para transferir la información de forma segura de manera interna, o bien quieres hacer pruebas antes de poner tu servidor web en producción, puedes crear un certificado SSL auto firmado. Por supuesto, para seguir estos pasos debes disponer de un [servidor Apache ya montado](https://tonisalm.github.io/2021/11/23/Configuraci%C3%B3n-b%C3%A1sica-de-Apache.html).

## Pasos

1. **Activar el módulo SSL:**

   ``` bash
   sudo a2enmod ssl
   sudo service apache2 restart
   ```

   

2. **Crear un certificado SSL auto firmado:**

   Para eso primero creamos un directorio para guardar la clave privada y el certificado, que generamos a continuación:

   ``` bash
   sudo mkdir /etc/apache2/ssl
   ```

   ![image-20211130163156004](/assets/images/image-20211130163156004.png)

   Las opciones indican, en este orden, que generamos un certificado X.509, sin proteger con contraseña (para que Apache no la pida cada vez que inicia), que expira al año, generando un par de claves RSA de 2048 bits, cuya clave privada guardará como "apache.key", y guardará el certificado como "apache.crt". Luego nos pide alguna información, donde lo importante es el nombre FQDN donde hay que poner el nombre de nuestro sitio.

   ![image-20211130163645344](/assets/images/image-20211130163645344.png)

   *Comprobamos que ha creado la clave y certificado*

   

3. **Configurar Apache:**

   Si vas a configurar un sitio desde cero, copia el archivo `/etc/apache2/sites-available/default-ssl.conf` al archivo de configuración del sitio que quieras configurar, p.ej. `website1.conf`, o bien, si ya tienes tu sitio configurado sin SSL, edita su fichero de configuración siguiendo los comentarios de la imagen:

   ![image-20211130165738908](/assets/images/image-20211130165738908.png)

   

4. **Configurar /etc/hosts:**

   Si no lo tienes hecho ya, añade tu sitio a /etc/hosts para que tu navegador pueda encontrarlo. En mi caso, la línea `127.0.0.1     website1.local`:

   ![image-20211130170333700](/assets/images/image-20211130170333700.png)

   

5. **Activar el host virtual:**

   Si no lo tienes ya activado, activa el sitio:

   ```bash
   sudo a2ensite website1.conf
   ```

   Y para que todo sea efectivo, reinicia el servidor:

   ```bash
   sudo service apache2 restart
   ```

   

6. **Probar que funciona:**

   Entra con el navegador a tu sitio web mediante HTTPS:

   ![image-20211130171246814](/assets/images/image-20211130171246814.png)

   Como verás, te dice que la conexión no es segura. Por supuesto, el navegador no confía en tu certificado autofirmado ya que no ha sido firmado por ninguna autoridad certificadora conocida. En "Avanzado" podemos añadir el sitio como excepción, de manera que no nos volverá a aparecer el aviso.

   Ahora ya tenemos HTTPS activado en nuestro sitio, pero como indicaba al principio, esto sólo nos servirá de manera interna o para pruebas. Si queremos sacar nuestra web al mundo, entonces tendremos que comprar un certificado emitido por una autoridad certificadora de confianza como [letsEncrypt.org](https://letsencrypt.org).

7. **Redirigir el tráfico a la versión HTTPS:**

   Normalmente nadie va a poner explícitamente "https://", así que redirigiremos el tráfico de HTTP (puerto 80) para que vaya al sitio HTTPS, modificando el archivo de configuración de nuestro sitio de la siguiente forma:

   ![image-20211130172441929](/assets/images/image-20211130172441929.png)

   Reiniciamos apache y ahora, cuando entremos en nuestro sitio, iremos directamente a la versión segura:

   ```bash
   sudo service apache2 restart
   ```

   ![image-20211130172843550](/assets/images/image-20211130172843550.png)

   *Como podemos ver en la primera línea de Firebug, se produce una redirección del sitio no seguro al sitio seguro*

