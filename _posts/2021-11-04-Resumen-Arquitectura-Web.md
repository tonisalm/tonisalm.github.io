---
typora-copy-images-to: ../assets/images
typora-root-url: ../
title: Resumen Arquitectura Web
---

# Resumen Arquitectura Web

## 1 Introducción

En informática, la World Wide Web (www) es un sistema de distribución de documentos electrónicos escritos en HTML (HyperText Markup Language) llamados páginas web, que se enlazan entre sí a través de hiperenlaces (links). Las páginas web pueden contener imágenes, textos, audio, vídeo y otros contenidos.

Normalmente las páginas web se agrupan en sitios web, compartiendo la primera parte de la dirección web (el dominio). Ejemplos:

- [https://www.ieselcaminas.org](http://www.ieselcaminas.org): Sitio web del Instituto
- [https://es.wikipedia.org](http://es.wikipedia.org): Sitio web de la Wikipedia en español

## 2 Aplicaciones web

### 2.1 ¿Qué es una aplicación web? Beneficios

Es una aplicación software que se utiliza accediendo a un servidor web mediante un navegador web. Sus beneficios respecto a las aplicaciones tradicionales son:

- Multiplataforma y multiusuario, aumentan su versatilidad.
- Gestión y mantenimiento centralizado, reducen costes.
- Datos centralizados, facilitan el acceso y el control.

### 2.2 Procesamiento de las aplicaciones web. Servidor.

Una aplicación web consiste en un conjunto de páginas web **estáticas** (se envían al navegador sin modificar su diseño original) y **dinámicas** (según la interacción con el usuario, son procesadas y modificadas antes de enviarlas).

![Fig.1 - Webs estaticas y dinámicas](https://emprendecontuweb.com/wp-content/uploads/2018/07/web-server-2.jpg)*Fig.1 - Webs estáticas y dinámicas ([fuente](https://emprendecontuweb.com/algunos-conceptos-basicos-en-diseno-web/))*

Cuando el servidor web recibe una petición para mostrar una página web dinámica, la transfiere al **servidor de aplicaciones**, que se encarga de buscar instrucciones en la página recibida, procesarlas y montar la página que se enviará finalmente al navegador.

#### 2.2.1 Páginas dinámicas con acceso a bases de datos

El uso de una base de datos para almacenar contenido permite separar el diseño del sitio web del contenido que se desea mostrar, de modo que sólo se necesita escribir una página para mostrar diferentes datos a diferentes usuarios.

Cuando se requieren datos de la base de datos, el servidor de aplicaciones realiza una consulta al controlador de la base de datos en lenguaje SQL y éste le devuelve los registros correspondientes, que se emplean para completar la página.

![Modelo servidor aplicaciones externo](https://javiergarciaescobedo.es/images/stories/despliegue_web/01_implantacion/Modelo_servidor_aplicaciones_externo.png)*Fig.2 - Acceso a base de datos ([fuente](https://javiergarciaescobedo.es/despliegue-de-aplicaciones-web/76-arquitecturas-web/253-modelos-de-arquitecturas-web))*

### 2.3. Visualización de páginas. Lado del cliente

Desde el punto de vista del **cliente web** (generalmente el navegador) las páginas que componen una aplicación web también se pueden dividir en estáticas y dinámicas.

- **Estáticas**: el contenido visualizado por el cliente no cambia (puede cambiar de página, pero no cambia la página en sí)
- **Dinámicas**: el contenido visualizado cambia dentro de la misma página, usando técnicas de manipulación del DOM y peticiones asíncronas (Single Page Application)

#### 2.3.1 Generadores de sitios estáticos (**SSG**) 

Los generadores de sitios estáticos funcionan convirtiendo texto simple y con formato ligero (generalmente [markdown](https://es.wikipedia.org/wiki/Markdown)) en sitios web, aplicando datos y contenido a las plantillas y generando una vista de una página, lo que facilita mucho servir el contenido, bien desde un servidor web simplificado o una red de distribución de contenido (CDN).

Además tienen una ventaja muy importante desde el punto de vista de la seguridad: simplifican mucho la infraestructura involucrada en servir el contenido, de modo que se minimizan los vectores de ataque.

![Flujo en SSG](https://victorponz.github.io/Ciberseguridad-PePS/assets/img/AWCG/ssg-host-flow.png)*Fig.3 - Flujo en SSG ([fuente](https://victorponz.github.io/Ciberseguridad-PePS/tema1/http/2020/11/04/Arquitectura-web-Conceptos-generales.html#34-aplicaci%C3%B3n-web))*

Algunos ejemplos de SSG son por ejemplo: [Jekill](http://jekyllrb.com/), [Hexo](https://hexo.io/), [Hugo](http://gohugo.io/), [Pelican](http://getpelican.com/), [Middleman](https://middlemanapp.com/), [Metalsmith](http://www.metalsmith.io/), [Ghost](https://ghost.org/)... Y una de las **mayores plataformas para desplegar** Stactic Site Generators es https://www.netlify.com/

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
