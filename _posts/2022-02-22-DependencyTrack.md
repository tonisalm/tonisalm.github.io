---
typora-copy-images-to: ../assets/images/2022-02-22-DependencyTrack/
typora-root-url: ../
title: Dependency Track
---

# Dependency Track

[DependencyTrack](https://dependencytrack.org/) es un proyecto de la OWASP que permite una gestión de las dependencias y de sus vulnerabilidades.

## Instalación

**IMPORTANTE: Necesitas 5GB de RAM o más para que funcione.**

Se puede instalar directamente con `docker-compose`:

```bash
curl -LO https://dependencytrack.org/docker-compose.yml
docker-compose up -d
```

En el archivo `docker-compose` se puede ver el puerto donde escucha el servicio (por defecto 8080). Abre el navegador y te pide login, por defecto "admin, admin", luego te pedirá cambiar la contraseña y al final llegas a la pantalla principal:

![image-20220222172100480](/assets/images/2022-02-22-DependencyTrack/image-20220222172100480.png)

## Creación de proyecto y carga de un SBOM

Para crear un proyecto, debemos cargar un archivo bom.xml. Puedes descargar un ejemplo de [aquí](https://github.com/CycloneDX/bom-examples/tree/master/SBOM) y luego cargarlo en Dependency Track. En primer lugar crea un proyecto nuevo entrando en "Projects > Create Project", luego entra en tu proyecto y añade el archivo bom.xml con "Upload BOM" y ordena por "Risk Score":

![image-20220222172645992](/assets/images/2022-02-22-DependencyTrack/image-20220222172645992.png)

![image-20220222173640435](/assets/images/2022-02-22-DependencyTrack/image-20220222173640435.png)

## Muestra información de vulnerabilidades

Pulsa en el botón "Audit Vulnerabilities" para mostrar las vulnerabilidades, y clica en alguna para ver de qué se trata:

![image-20220222174218810](/assets/images/2022-02-22-DependencyTrack/image-20220222174218810.png)
