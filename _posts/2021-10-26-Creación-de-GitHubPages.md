---
typora-copy-images-to: ../assets/images
typora-root-url: ../
---

# Creación de GitHub Pages

**Nota:** Si todavía no tienes tu sitio en GitHub, tendrás que crearlo antes de seguir estas instrucciones.

## Creación del repositorio

1. Entra en tu cuenta de [Github](https://github.com) y crea un repositorio nuevo pulsando en el "+" arriba a la derecha y en "New repository"

   ![image-20211026170915346](/assets/images/image-20211026170915346.png)

2. En el nombre del repositorio usa el formato `username.github.io` donde `username` es tu nombre usuario de `GitHub` 

   ![image-20211026171022781](/assets/images/image-20211026171022781.png)

   *En mi caso me dice que ya existe porque ya lo tengo creado*

3. Pulsa el botón "Create repository"

   ![image-20211026170957349](/assets/images/image-20211026170957349.png)

## Clonar el repositorio para trabajar localmente

1. En tu equipo, ve a la carpeta donde quieras poner tu repositorio y abre un terminal, por ejemplo en `~/Documentos/Repo`

2. En el terminal, clona el repositorio con el comando

   `git clone https://github.com/username/username.github.io`

   *(cambia "username" por tu nombre de usuario)*

## Crea tu página personal

1. En el terminal, ve al directorio local del proyecto, por ejemplo:
   `cd ~/Documentos/Repo/tonisalm.github.io`
2. Escribe tu megafantástica página:
   `echo "Mi primera página güé" > index.html`

## Sube tu megafantástica página para que la vean todos

1. En el terminal, dentro de tu directorio local del proyecto, escribe lo siguiente:

   ```
   git add *
   git commit -m "Primer commit"
   git push
   ```

   *Te pedirá tus datos de usuario y token para subir el contenido*

2. Si todo ha ido bien, ve al navegador e introduce la dirección de tu proyecto, por ejemplo: [https://tonisalm.github.io](https://tonisalm.github.io)
