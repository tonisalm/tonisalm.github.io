---
typora-copy-images-to: ../assets/images/2022-05-10-Practica-ASVS/
typora-root-url: ../
title: Práctica ASVS
---

# Práctica ASVS

## Descripción de la aplicación web

A través de la aplicación web de Industrias Fols, los clientes podrán gestionar su marca personalizada. Tendrán acceso a los diferentes embalajes y etiquetas, que podrán personalizar con sus imágenes y textos en formatos normalizados.

## Application Security Verification Level

Siguiendo [OWASP Application Security Verification Standard](https://raw.githubusercontent.com/OWASP/ASVS/v4.0.3/4.0/OWASP%20Application%20Security%20Verification%20Standard%204.0.3-en.pdf), el nivel objetivo de la aplicación es el nivel 1.

## V1 Architecture, Design and Threat Modeling

*1.1.1 Verify the use of a secure software development lifecycle that addresses security in all stages of development.*

- [ ] Se debe usar una metodología de desarrollo que tenga en cuenta la seguridad en todas sus fases.

## V2 Authentication

*2.3.1 Verify system generated initial passwords or activation codes SHOULD be securely randomly generated, SHOULD be at least 6 characters long, and MAY contain letters and numbers, and expire after a short period of time. These initial secrets must not be permitted to become the long term password.*

- [x] La generación automática de contraseñas debe ser aleatoria y segura, siendo de al menos 6 caracteres, pudiendo contener letras y números y con un corto periodo de expiración.

## V3 Session Management

*3.3.2 If authenticators permit users to remain logged in, verify that re-authentication occurs periodically both when actively used or after an idle period.*

- [x] La sesión no puede permanecer activa durante más de 30 días, sin tener en cuenta el tiempo de inactividad.

## V4 Access Control

*4.1.3 Verify that the principle of least privilege exists - users should only be able to access functions, data files, URLs, controllers, services, and other resources, for which they possess specific authorization. This implies protection against
spoofing and elevation of privilege.*

- [x] Verificar que se aplica el principio de mínimo privilegio. Por defecto los usuarios tendrán autorización para el acceso a un número limitado de recursos, debiendo otorgar permisos expresamente para acceder a otros recursos.

## V5 Validation, Sanitization and Encoding

*5.2.5 Verify that the application protects against template injection attacks by ensuring that any user input being included is sanitized or sandboxed.*

- [x] Verificar que la aplicación está protegida contra ataques de inyección de código, asegurándose de que cualquier entrada del usuario está saneada o enjaulada.

## V6 Stored Cryptography

*6.2.1 Verify that all cryptographic modules fail securely, and errors are handled in a way that does not enable Padding Oracle attacks.*

- [x] Verificar que los módulos criptográficos manejan los errores de forma segura, evitando ataques *Padding Oracle*.

## V7 Error Handling and Logging

*7.1.1 Verify that the application does not log credentials or payment details. Session tokens should only be stored in logs in an irreversible, hashed form.*

- [x] Verificar que la aplicación no guarda información sensible como credenciales o detalles de pago. Los tokens de sesión almacenados en logs no se deben ver en claro, sólo su hash.

## V8 Data Protection

*8.2.2 Verify that data stored in browser storage (such as localStorage, sessionStorage, IndexedDB, or cookies) does not contain sensitive data.*

- [x] Verificar que los datos almacenados en el navegador no contienen datos sensibles.

## V9 Communication

*9.1.3 Verify that only the latest recommended versions of the TLS protocol are enabled, such as TLS 1.2 and TLS 1.3. The latest version of the TLS protocol should be the preferred option.*

- [x] Verificar que sólo se usan versiones actualizadas del protocolo TLS, preferiblemente la más reciente.

## V10 Malicious Code

*10.3.1 Verify that if the application has a client or server auto-update feature, updates should be obtained over secure channels and digitally signed. The update code must validate the digital signature of the update before installing or executing the update.*

- [x] Verificar que la aplicación se actualiza automáticamente y que las actualizaciones se obtienen desde canales seguros. El código de actualización debe comprobar la firma digital de la actualización antes de aplicarla.

## V11 Business Logic

*11.1.4 Verify that the application has anti-automation controls to protect against excessive calls such as mass data exfiltration, business logic requests, file uploads or denial of service attacks.*

- [x] Verificar que la aplicación dispone de controles anti automatización para protegerse contra llamadas, entradas o salidas de datos masivas y ataques de denegación de servicio.

## V12 Files and Resources

*12.3.1 Verify that user-submitted filename metadata is not used directly by system or framework filesystems and that a URL API is used to protect against path traversal.*

- [x] Verificar que los metadatos de los archivos subidos por el usuario no son utilizados directamente por el sistema del servidor, y que se aplican protecciones de URL para evitar ataques del tipo *path traversal*.

## V13 API and Web Service

*13.3.1 Verify that XSD schema validation takes place to ensure a properly formed XML document, followed by validation of e path traversal.ach input field before any processing of that data takes place.*

- [x] Verificar que se ejecutan validaciones de esquema y campos para documentos XML antes de procesarlos.

## V14 Configuration

*14.2.1 Verify that all components are up to date, preferably using a dependency checker during build or compile time.*

- [x] Verificar que todos los componentes están actualizados, usando un comprobador de dependencias durante la fase de construcción o compilación.