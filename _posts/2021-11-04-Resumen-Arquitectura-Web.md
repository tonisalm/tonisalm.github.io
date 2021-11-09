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

## 3 Aplicaciones Web

### 3.1 ¿Qué es la web?

Según [Wikipedia](https://es.wikipedia.org/wiki/World_Wide_Web),

> En informática, la World Wide Web (WWW) o red informática mundial  es un sistema de distribución de documentos de hipertexto o hipermedios  interconectados y accesibles vía Internet. Con un navegador web, un  usuario visualiza sitios web compuestos de páginas web que pueden  contener textos, imágenes, vídeos u otros contenidos multimedia, y  navega a través de esas páginas usando hiperenlaces.

### 3.2 Página web

Una página web es un documento electrónico escrito en **HTML** (HyperText Markup Language). Las páginas web están enlazadas a través  de hiperenlaces (links). Mediante un navegador un usuario puede navegar a través de la web siguiendo los hiperenlaces

Las páginas web enlazan contenidos de naturaleza heterogénea:

- Imágenes: JPG, GIF, PNG, …
- Documentos: PDF, TXT, …
- Audio: MP3, WAV, …
- Vídeo: AVI, MPEG, …

## 4 Tecnologías de desarrollo Web

Como hemos comentado en el apartado Aplicaciones Web, hoy en día se  utilizan páginas dinámicas tanto del lado del servidor como del cliente.

El impacto de la Web ha propiciado la aparición de una gran cantidad  de tecnologías, librerías, herramientas y estilos arquitectónicos para  desarrollar una aplicación web. Para facilitar la tarea de escoger cuál  es la tecnología más adecuada para un proyecto, es conveniente conocer  los elementos más importantes desde un punto de vista de alto nivel para tener una visión global de la programación web.

Existen dos enfoques en el desarrollo de aplicaciones web:

- Creación de aplicaciones web con [integración de tecnologías de desarrollo](https://victorponz.github.io/Ciberseguridad-PePS/tema1/http/2020/11/04/Arquitectura-web-Conceptos-generales.html#41-integración-de-tecnologías-de-desarrollo)
- Creación de aplicaciones web con [sistemas gestores de contenido](https://victorponz.github.io/Ciberseguridad-PePS/tema1/http/2020/11/04/Arquitectura-web-Conceptos-generales.html#42-sistemas-gestores-de-contenido-cms)

### 4.2 Sistemas gestores de contenido (CMS)

Existen aplicaciones web cuya principal funcionalidad es la  publicación de contenido: blogs, páginas de empresas, organismos  públicos, comercio electrónico, etc.

Todas estas webs tienen mucho en común, prácticamente sólo se diferencian en el contenido y en el aspecto gráfico.

Para desarrollar este tipo de webs, en vez de desarrollar la web con  integración de tecnologías de desarrollo se usa una aplicación ya creada que se puede personalizar y adaptar (mayormente vía web).

A las aplicaciones de este tipo se las denomina **Sistemas Gestores de Contenido** (*Content Managemenet Systems*).

Entre las **ventajas** de contar con un CMS están:

1. **Fácilmente actualizable**. La edición del contenido está separada del diseño y funcionalidad del sitio. Esto permite  introducir contenido a usuarios con poca formación técnica sin  interferir en el diseño.
2. A cada usuario se le pueden asignar permisos de acceso selectivo basados en sus **roles** (por ejemplo, puede permitir que algunos usuarios solo agreguen y edite su propio contenido, mientras que otros le dan acceso universal).
3. **Uso de plantillas totalmente personalizables.**Tanto para el contenido como para el diseño de tu propio sitio web. Esta es  una de las partes que más suelen asustar si no se tienen conocimientos.  Pero ahora no hace falta ser un experto en diseño.
4. Los componentes básicos de la aplicación como menús, cabeceras,  pies de página y laterales se mantienen desde la interfaz de  administración.
5. Además, **los mejores CMS incluyen plugins** para poder sacar aún más partido a todas las funcionalidades

Evidentemente, no son todo ventajas. Los principales **inconvenientes** de usar un CMS son:

1. Desafortunadamente, como el código fuente está disponible al público en general y *todas las aplicaciones tienen bugs* de seguridad, hay algunos hackers malévolos por ahí que pueden  averiguar cómo entrar en estas plataformas; por lo que la seguridad  requerirá precauciones adicionales.
2. Hacer que el sitio web se vea exactamente como queremos puede suponer desafío. Esto es cierto en algunos *frameworks* de CMS más que otros, pero todos presentan un poco más de trabajo para definir un ‘estilo’ propio.
3. El CMS almacena todo por separado, luego lo monta en el momento en que el cliente web solicita una página, lo que significa que pueden ser lentos; sin embargo, esto se puede mitigar mediante el uso de un  almacenamiento en [caché](https://es.wikipedia.org/wiki/Caché_web) fuerte y eficaz y con una [Red de Entrega de Contenidos](https://es.wikipedia.org/wiki/Red_de_entrega_de_contenidos) (*Content Delivery Network* - CDN).
4. Limitaciones de funcionalidad: Hay algunas cosas que no se pueden hacer en un CMS, al menos no sin reescribir parte del código.
