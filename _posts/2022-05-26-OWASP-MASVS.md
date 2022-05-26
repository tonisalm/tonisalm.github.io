---
typora-copy-images-to: ../assets/images/2022-05-26-OWASP-MASVS/
typora-root-url: ../
title: OWASP MASVS
---

# OWASP MASVS

## V1: Requisitos de Arquitectura, Diseño y Modelado de Amenazas

**1.2 MSTG-ARCH-2 Los controles de seguridad nunca se aplican sólo en el cliente, sino que también en los respectivos servidores.**

*Hay que aplicar la seguridad tanto en el cliente como en el servidor (y en las comunicaciones, aunque no lo diga).*

**1.12 MSTG-ARCH-12 La aplicación debería de cumplir con las leyes y regulaciones de privacidad.**

*Hay que estudiarse bien las leyes de protección de datos para asegurarse de que no incumple alguna.*

## V2: Requerimientos de Almacenamiento de datos y Privacidad

**2.2 MSTG-STORAGE-2 No se debe almacenar información sensible fuera del contenedor de la aplicación o del almacenamiento de credenciales del sistema.**

*Esto es de lógica, la información sensible debe estar perfectamente controlada.*

**2.7 MSTG-STORAGE-7 No se expone información sensible como contraseñas y números de tarjetas de crédito a través de la interfaz o capturas de pantalla.**

*Por si acaso te están espiando.*

## V3: Requerimientos de Criptografía

**3.1 MSTG-CRYPTO-1 La aplicación no depende únicamente de criptografía simétrica cuyas claves se encuentran directamente en el código fuente de la misma.**

*Nunca se deben almacenar claves directamente en el código, por si hacen ingeniería inversa.*

**3.5 MSTG-CRYPTO-5 La aplicación no reutiliza una misma clave criptográfica para varios propósitos.**

*Si por casualidad te pillan una clave, que no lo tengan todo.*

## V4: Requerimientos de Autenticación y Manejo de Sesiones
**4.6 MSTG-AUTH-6 El servidor implementa mecanismos, cuando credenciales de autenticación son ingresadas una cantidad excesiva de veces.**

*Es básico protegerse contra ataques de fuerza bruta y denegación de servicio.*

**4.7 MSTG-AUTH-7 Las sesiones y los tokens de acceso expiran luego de un tiempo predefinido de inactividad.**

*Con esta limitación se dificulta el robo y uso fraudulento de un token.*

## V5: Requerimientos de Comunicación a través de la red

**5.1 MSTG-NETWORK-1 La información es enviada cifrada utilizando TLS. El canal seguro es usado consistentemente en la aplicación.**

*Es básico proteger las comunicaciones. TLS es el protocolo estándar de comunicación cifrada.*

**5.3 MSTG-NETWORK-3 La aplicación verifica el certificado X.509 del sistema remoto al establecer el canal seguro y sólo se aceptan certificados firmados por una CA de confianza.**

*Así la aplicación se asegura de que se está conectando al servidor correcto. Evita ataques MIM.*

## V6: Requerimientos de Interacción con la Plataforma

**6.2 MSTG-PLATFORM-2 Todo dato ingresado por el usuario o cualquier fuente externa debe ser validado y, si es necesario, saneado. Esto incluye información recibida por la UI o mecanismos IPC como los Intents, URLs y datos provenientes de la red.**

*Para evitar ataques de inyección de código.*

**6.9 MSTG-PLATFORM-9 La aplicación se protege contra ataques de tipo screen overlay. (sólo Android).**

*Para evitar la captura de datos introducidos por el usuario.*

## V7: Requerimientos de Calidad de Código y Configuración del Compilador

**7.4 MSTG-CODE-4 Cualquier código de depuración y/o de asistencia al desarrollador (p. ej. código de test, backdoors, configuraciones ocultas) debe ser eliminado. La aplicación no hace logs detallados de errores ni de mensajes de  depuración.**

*Así la aplicación está limpia y no proporciona pistas sobre su código con los mensajes de depuración.*

**7.5 MSTG-CODE-5 Todos los componentes de terceros se encuentran identificados y revisados en cuanto a vulnerabilidades conocidas.**

*Esto es básico para cualquier aplicación de cualquier tipo.*

## V8: Requerimientos de Resistencia ante la Ingeniería Inversa

**8.3 MSTG-RESILIENCE-3 La aplicación detecta y responde a cualquier modificación de ejecutables y datos críticos de la propia aplicación.**

*Verificando la suma de los archivos, puede detectar manipulaciones no deseadas y asegurarse de que se ejecuta como se espera.*

**8.9 MSTG-RESILIENCE-9 La ofuscación se aplica a las defensas del programa, lo que a su vez impide la desofuscación mediante análisis dinámico.**

*Se aplica la ofuscación de código para que no se pueda ver el código original.*
