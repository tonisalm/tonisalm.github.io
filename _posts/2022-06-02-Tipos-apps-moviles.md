---
typora-copy-images-to: ../assets/images/2022-06-02-Tipos-apps-moviles/
typora-root-url: ../
title: Tipos de aplicaciones móviles
---

# Tipos de aplicaciones móviles

## Native App

Una aplicación nativa es aquella desarrollada para usar en una plataforma en particular. Su mayor ventaja es que se aprovecha del uso directo del hardware y software sobre el que corre, por lo que su rendimiento puede ser superior y aprovechar al máximo la tecnología del dispositivo. También pueden realizar tareas en segundo plano y enviar notificaciones. Su mayor inconveniente es que hay que desarrollar la aplicación para cada plataforma donde se quiera utilizar.

## Web App

Una aplicación web básicamente es una aplicación alojada en un servidor web, a la que se accede a través de un navegador web estándar desde prácticamente cualquier sistema operativo.

## Hybrid App

Es una mezcla entre aplicación nativa y aplicación web, es decir, te instalas una aplicación nativa que utiliza principalmente una aplicación web por debajo, pero además realiza acciones sobre el dispositivo que una aplicación web no podría realizar, como acceder directamente a la cámara o al gps (una aplicación web puede hacerlo, pero no directamente si no a través del navegador).

## PWA app

Las Progressive Web Apps son aplicaciones web que se instalan como si fuesen una aplicación más en el dispositivo, aunque en realidad no instalan nada. Aunque no dejan de ser aplicaciones web (tampoco son aplicaciones híbridas), gracias a los *service workers* y otras tecnologías, son capaces de ejecutarse en segundo plano sin necesidad de tener el navegador abierto. Obviamente el sistema operativo debe soportar este tipo de tecnología, ya que, simplificando mucho, es como una orden que hace que el sistema siempre tenga instancias del navegador abiertas para estas aplicaciones, aunque no las estés utilizando en ese momento.

La mayor ventaja de estas aplicaciones web, es que, una vez cargadas, puedes utilizarlas sin conexión a Internet o recibir notificaciones sin tenerla abierta, por lo que su uso, a la práctica, se asemeja a una aplicación nativa.
