---
typora-copy-images-to: ../assets/images
typora-root-url: ../
title: Resumen Arquitectura Web
---

# Resumen Arquitectura Web

## 1 Introducción

En este tema trataremos los conceptos generales de una aplicación  web, así como las tendencias actuales relativas a las tecnologías más  usadas en el desarrollo de aplicaciones web dinámicas.

## 2 Conceptos generales de arquitectura web

En esta unidad aprenderemos los siguientes conceptos.

1. Qué es una aplicación web
2. Qué tecnologías se pueden utilizar para crear una aplicación web
3. Qué es un servidor web
4. Y, finalmente, en qué consiste el despliegue de una aplicación Web

## 5 Servidores web y de aplicaciones

Si se escoje implementar la aplicación con Java se usará un servidor de aplicaciones, mientras si se escoje PHP o ASP.NET se usará un servidor web. A continuación se definirán los conceptos de servidor web y servidor de aplicaciones:

### 5.1 Servidor Web

El servidor web controla el protocolo http. Si el servidor web recibe una solicitud por protocolo http responderá con una respuesta http.

El servidor web no proporiciona ninguna funcionalidad más que proporicionar un entorno en el que el programa del lado del servidor puede ejecutar y devolver las respuestas generadas.

### 5.2 Servidor de aplicaciones

El servidor de aplicaciones proporiciona acceso a la lógica empresarial para su uso por los programas de aplicación del cliente.

## 6 Despliegue de Aplicaciones web

Cuando se crea una aplicación weben el ordenador se suele hacer en entorno de desarrollo al que solo se puede acceder desde el mismo ordenador.

En el entorno de desarrollo local o localhost el codigo de la aplicación se mantiene en el ordenador y será accesible únicamente desde el mismo ordenador. Si se desde que sea accesible por todo el mundo se necesita ponerla en un ordenador públlico, accesible a trevés de una URL.

El proceso de mover una aplicación web desde localhost a un servidor web se denomina despliegue.

En el momento que se desplega la aplicación en un servidor web se dispone de una versión públicamente disponible para cualquier usuario y al mismo tiempo se sigue disponiendo de la versión privada donde se usa para retocar y probar posible nuevo contenido.

## Caso real de despliege de aplicaciones web (Storyblocks)

![web-arch](/assets/images/web-arch.png)



En este caso se excpone el proceso de busqueda en google de una imagen donde en el navegador se mustra como primera opción Storyblocks, si el usuario eliga está opción entonces el usuario envia una solicitud al servidor DNS para buscar como conectarse al sitio web. La silicitud enviada llega al balanceador de carga  para elegir el servidor más adecuado del sitio web. El servidor muestra la vista como html y luego la envía al navegador del usuario pasando a través del equilibrador de carga y al final muestra la página al usuario.

A continuación se expone la estructura de la aplicación web Storyblocks:

- DNS: Que proporciona una búsqueda de clave / valor  desde un nombre de dominio a una dirección IP, ya que la dns es necesaria para que el ordenador del usuario enrute una solicitud al servidor de la página de busqueda.

- Balanceador de carga: Enrutan las solicitudes entrantes a uno de los servidores de la aplicación web y envían la respuesta desde el servidor de la aplicación al usuario.

- Servidores de aplicaciones web: Reciben la solicitud de un usuario y envían de vuelta un html al navegador del usuario que realiza la busqueda.

- Servidores de bases de datos: Es donde la aplicación web almacena la información que ha solicitado el usuario a la hora de realizar la busqueda.

- Servicio de caché: Se aprovecha para guardar los resultados de los cálculos costosos.

- Servicio de búsqueda de texto completo: Es una función que el servidor de la aplicación web ofrece para facilitar la busqueda del usuario e forma más rápida.
- Servicios.
- Datos.
- Almacenamiento en la nube: Es una forma para almacenar las entradas de busqueda de los usuarios para posibles futuras busquedas de los mismos.



---



