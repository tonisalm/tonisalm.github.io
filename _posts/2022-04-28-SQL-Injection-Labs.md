---
typora-copy-images-to: ../assets/images/2022-04-28-SQL-Injection-Labs/
typora-root-url: ../
title: SQL Injection Labs
---

# SQL Injection Labs

En esta práctica vamos a realizar una serie de laboratorios de Web Security Academy, donde practicaremos ataques de SQL Injection. En primer lugar hay que registrarse en la página y acceder a los laboratorios, donde tienes que modificar la URL con los parámetros adecuados para mostrar el resultado que se pide.

## [Lab 1: WHERE clause](https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data)

![image-20220428174452709](/assets/images/2022-04-28-SQL-Injection-Labs/image-20220428174452709.png)

Simplemente añade `'+OR+1=1--` al final de la URL.

## [Lab 2: login bypass](https://portswigger.net/web-security/sql-injection/lab-login-bypass)

![image-20220428180021082](/assets/images/2022-04-28-SQL-Injection-Labs/image-20220428180021082.png)

Accede a "My Account". En el formulario introduce el usuario `administrator'--` y cualquier password.

## [Lab 3: SQL injection UNION attack](https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns)

![image-20220428180813841](/assets/images/2022-04-28-SQL-Injection-Labs/image-20220428180813841.png)

Pulsa en una categoría y añade al final de la URL el texto `'+UNION+SELECT+NULL,NULL--` (es muy raro que sólo tenga una columna), da un error, añade un NULL más y ya lo tienes. Si hubiese dado error, seguiríamos añadiendo NULLs hasta dar con el número de columnas de la tabla.

## [Lab 4: Database version](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle)

![image-20220428183227322](/assets/images/2022-04-28-SQL-Injection-Labs/image-20220428183227322.png)

Pulsa en una categoría y haz como en el laboratorio anterior para averiguar las columnas. Después, consulta el campo banner de la tabla v$version para mostrar la versión de la base de datos Oracle: `'+UNION+SELECT+BANNER,NULL+FROM+v$version--`

